# Use Cases & Minimal Tests — McDonald's Social Media Command Centre

Each use case lists the critical path and the minimal tests needed to verify it.

---

## UC-01 · Load and clean the dataset

**Actor:** System (on first app load)  
**Path:** `kagglehub.dataset_download()` → `get_df()` → cached DataFrame

**Critical assertions:**
- `rating` column contains only integers 1–5 (no strings like `"4 stars"`)
- `street`, `city`, `zip`, `country` columns all exist after address parsing
- `total_days_ago` is a non-negative float
- No null rows remain in the returned DataFrame

```python
# tests/test_data_loader.py
import pandas as pd
from app.data_loader import get_df

def test_rating_is_numeric():
    df = get_df()
    assert pd.api.types.is_integer_dtype(df["rating"]) or pd.api.types.is_float_dtype(df["rating"])
    assert df["rating"].between(1, 5).all()

def test_address_columns_exist():
    df = get_df()
    for col in ["street", "city", "zip", "country"]:
        assert col in df.columns, f"Missing column: {col}"

def test_total_days_ago_non_negative():
    df = get_df()
    assert (df["total_days_ago"] >= 0).all()

def test_no_nulls():
    df = get_df()
    assert df.isnull().sum().sum() == 0
```

---

## UC-02 · Filter reviews by branch and time window

**Actor:** Co-Pilot tab (on branch select or slider change)  
**Path:** `filter_reviews(df, street, days)` → filtered DataFrame

**Critical assertions:**
- Only rows where `street` exactly matches the given value are returned
- All returned rows have `total_days_ago <= days`
- Returns empty DataFrame (not an error) when no rows match

```python
# tests/test_data_loader.py
import pandas as pd
from app.data_loader import filter_reviews

def _sample_df():
    return pd.DataFrame({
        "street": ["123 Main St", "123 Main St", "456 Elm St"],
        "total_days_ago": [10.0, 100.0, 5.0],
        "rating": [3, 4, 5],
        "review": ["ok", "good", "great"],
    })

def test_filter_correct_street():
    result = filter_reviews(_sample_df(), "123 Main St", 90)
    assert (result["street"] == "123 Main St").all()

def test_filter_respects_day_window():
    result = filter_reviews(_sample_df(), "123 Main St", 90)
    assert (result["total_days_ago"] <= 90).all()
    assert len(result) == 1  # only the 10-day row passes

def test_filter_returns_empty_not_error():
    result = filter_reviews(_sample_df(), "999 Unknown St", 90)
    assert isinstance(result, pd.DataFrame)
    assert result.empty
```

---

## UC-03 · Format reviews for the LLM

**Actor:** `llm.py` internal — called inside `generate_insights()`  
**Path:** `_format_reviews(df)` → numbered string of top-20 reviews

**Critical assertions:**
- Output is capped at 20 reviews regardless of input size
- Each line follows `N. [rating/5] review_text` format
- Reviews are sorted most-recent first (`total_days_ago` ascending)

```python
# tests/test_llm.py
import pandas as pd
from app.llm import _format_reviews

def _make_df(n: int) -> pd.DataFrame:
    return pd.DataFrame({
        "rating": [4] * n,
        "review": [f"review {i}" for i in range(n)],
        "total_days_ago": list(range(n)),
    })

def test_capped_at_20():
    result = _format_reviews(_make_df(50))
    lines = [l for l in result.strip().splitlines() if l.strip()]
    assert len(lines) == 20

def test_format_structure():
    result = _format_reviews(_make_df(3))
    lines = result.strip().splitlines()
    assert lines[0].startswith("1. [4/5]")
    assert lines[1].startswith("2. [4/5]")

def test_sorted_most_recent_first():
    df = _make_df(5)  # days_ago = 0,1,2,3,4 → row 0 is most recent
    result = _format_reviews(df)
    first_line = result.strip().splitlines()[0]
    assert "review 0" in first_line
```

---

## UC-04 · Generate AI insights via Claude API

**Actor:** Manager (clicks "Generate Insights" button)  
**Path:** `generate_insights(df, branch, days)` → structured response string

**Critical assertions:**
- Returns a non-empty string on success
- Returns a user-friendly error string (not an exception) on `AuthenticationError`
- Returns a user-friendly error string on `APIConnectionError`
- Empty DataFrame input returns early without making an API call

```python
# tests/test_llm.py
from unittest.mock import patch, MagicMock
import pandas as pd
import anthropic
from app.llm import generate_insights

def _reviews():
    return pd.DataFrame({
        "rating": [3, 4],
        "review": ["slow service", "good food"],
        "total_days_ago": [1.0, 2.0],
    })

def test_empty_df_returns_early():
    result = generate_insights(pd.DataFrame(), "Test Branch", 90)
    assert "No reviews" in result

def test_returns_string_on_success():
    mock_msg = MagicMock()
    mock_msg.content = [MagicMock(text="SUMMARY\nOK\nACTION ITEMS\n1. Fix\nSTAFF BRIEFING\nTeam, do better.")]
    with patch("app.llm.anthropic.Anthropic") as MockClient:
        MockClient.return_value.messages.create.return_value = mock_msg
        result = generate_insights(_reviews(), "Test Branch", 90)
    assert isinstance(result, str)
    assert len(result) > 0

def test_auth_error_returns_message():
    with patch("app.llm.anthropic.Anthropic") as MockClient:
        MockClient.return_value.messages.create.side_effect = anthropic.AuthenticationError(
            message="invalid key", response=MagicMock(), body={}
        )
        result = generate_insights(_reviews(), "Test Branch", 90)
    assert "Authentication failed" in result

def test_connection_error_returns_message():
    with patch("app.llm.anthropic.Anthropic") as MockClient:
        MockClient.return_value.messages.create.side_effect = anthropic.APIConnectionError(
            request=MagicMock()
        )
        result = generate_insights(_reviews(), "Test Branch", 90)
    assert "Could not reach" in result
```

---

## UC-05 · Parse Claude response into sections

**Actor:** `app.py` — called after `generate_insights()` returns  
**Path:** `_parse_sections(result)` → `{"SUMMARY": ..., "ACTION ITEMS": ..., "STAFF BRIEFING": ...}`

**Critical assertions:**
- All three keys are present in the output dict
- Each section body contains the correct text
- Case-insensitive matching (handles `summary` or `Summary`)
- Missing sections return an empty string, not `None` or an error

```python
# tests/test_app.py
import sys
from pathlib import Path
sys.path.insert(0, str(Path(__file__).resolve().parent.parent / "app"))
from app import _parse_sections

_SAMPLE = """SUMMARY
Service was slow.

ACTION ITEMS
1. Retrain staff — Evidence: "waited 20 min" — Action: run daily briefing

STAFF BRIEFING
Team, please speed up the drive-through."""

def test_all_keys_present():
    result = _parse_sections(_SAMPLE)
    assert set(result.keys()) == {"SUMMARY", "ACTION ITEMS", "STAFF BRIEFING"}

def test_section_bodies_correct():
    result = _parse_sections(_SAMPLE)
    assert "Service was slow" in result["SUMMARY"]
    assert "Retrain staff" in result["ACTION ITEMS"]
    assert "Team, please" in result["STAFF BRIEFING"]

def test_case_insensitive():
    lowered = _SAMPLE.lower()
    result = _parse_sections(lowered)
    assert result["SUMMARY"] != ""

def test_missing_section_returns_empty_string():
    result = _parse_sections("SUMMARY\nOnly this section.")
    assert result["ACTION ITEMS"] == ""
    assert result["STAFF BRIEFING"] == ""
```

---

## UC-06 · Branch health table computation

**Actor:** Co-Pilot tab (on load)  
**Path:** `_build_health_table(df)` → summary DataFrame with Status and Trend columns

**Critical assertions:**
- Status is `🔴 Needs Attention` when avg rating < 2.5
- Status is `🟡 Monitor` when avg rating is between 2.5 and 3.5
- Status is `🟢 Healthy` when avg rating > 3.5
- Trend is `⬆️` when last-30-day avg exceeds 31–60-day avg by more than 0.1
- Trend is `⬇️` when last-30-day avg is more than 0.1 below 31–60-day avg
- Trend falls back to `➡️` when prior-period data is absent

```python
# tests/test_app.py
import pandas as pd
import sys
from pathlib import Path
sys.path.insert(0, str(Path(__file__).resolve().parent.parent / "app"))
from app import _build_health_table

def _df(street, ratings, days_ago_list):
    return pd.DataFrame({
        "street": [street] * len(ratings),
        "city": ["TestCity"] * len(ratings),
        "rating": ratings,
        "total_days_ago": days_ago_list,
    })

def test_status_red():
    df = _df("Bad St", [1, 2, 1], [5, 10, 15])
    result = _build_health_table(df)
    assert result.loc[result["Branch"] == "Bad St", "Status"].values[0] == "🔴 Needs Attention"

def test_status_yellow():
    df = _df("Mid St", [3, 3, 3], [5, 10, 15])
    result = _build_health_table(df)
    assert result.loc[result["Branch"] == "Mid St", "Status"].values[0] == "🟡 Monitor"

def test_status_green():
    df = _df("Good St", [5, 5, 5], [5, 10, 15])
    result = _build_health_table(df)
    assert result.loc[result["Branch"] == "Good St", "Status"].values[0] == "🟢 Healthy"

def test_trend_up():
    # recent (≤30): avg 5.0 — prior (31-60): avg 3.0 → diff +2.0 > 0.1
    df = _df("Up St", [5, 5, 3, 3], [10, 20, 40, 50])
    result = _build_health_table(df)
    assert result.loc[result["Branch"] == "Up St", "Trend"].values[0] == "⬆️"

def test_trend_down():
    # recent: avg 2.0 — prior: avg 5.0 → diff -3.0 < -0.1
    df = _df("Down St", [2, 2, 5, 5], [10, 20, 40, 50])
    result = _build_health_table(df)
    assert result.loc[result["Branch"] == "Down St", "Trend"].values[0] == "⬇️"

def test_trend_stable_no_prior_data():
    df = _df("Stable St", [4, 4], [5, 10])  # no rows in 31–60 range
    result = _build_health_table(df)
    assert result.loc[result["Branch"] == "Stable St", "Trend"].values[0] == "➡️"
```

---

## UC-07 · LLM call logging to SQLite

**Actor:** System — called inside `generate_insights()` after a successful API response  
**Path:** `_log(branch, days_range, num_reviews, response_text)` → row inserted in `llm_logs.db`

**Critical assertions:**
- A row is inserted with the correct field values
- `timestamp` is a valid ISO-8601 string
- `num_reviews` is capped at 20 even if more reviews were passed

```python
# tests/test_llm.py
import sqlite3, tempfile, os
from pathlib import Path
from unittest.mock import patch
from app.llm import _log

def test_log_inserts_row():
    with tempfile.TemporaryDirectory() as tmpdir:
        db_path = Path(tmpdir) / "test_logs.db"
        with patch("app.llm._DB_FILE", db_path):
            _log("123 Main St", 90, 15, "some response")
            conn = sqlite3.connect(db_path)
            row = conn.execute("SELECT * FROM llm_logs").fetchone()
            conn.close()
        assert row is not None
        assert row[2] == "123 Main St"   # branch
        assert row[3] == 90              # days_range
        assert row[4] == 15             # num_reviews
        assert row[5] == "some response" # response_text

def test_log_timestamp_is_iso():
    from datetime import datetime
    with tempfile.TemporaryDirectory() as tmpdir:
        db_path = Path(tmpdir) / "test_logs.db"
        with patch("app.llm._DB_FILE", db_path):
            _log("Test St", 30, 5, "resp")
            conn = sqlite3.connect(db_path)
            ts = conn.execute("SELECT timestamp FROM llm_logs").fetchone()[0]
            conn.close()
        datetime.fromisoformat(ts)  # raises ValueError if not valid ISO
```

---

## Running the Tests

```bash
# From project root
pip install pytest
pytest tests/ -v
```

> **Note:** UC-01 (`get_df`) requires a valid Kaggle token (`~/.kaggle/kaggle.json`). All other tests use synthetic DataFrames and mocked API calls — no external credentials needed.
