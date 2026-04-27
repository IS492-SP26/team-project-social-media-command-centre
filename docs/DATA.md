# Dataset Documentation

## McDonald's Store Reviews

| Field | Detail |
|-------|--------|
| **Name** | McDonald's Store Reviews |
| **Source** | Kaggle — [nelgiriyewithana/mcdonalds-store-reviews](https://www.kaggle.com/datasets/nelgiriyewithana/mcdonalds-store-reviews) |
| **Size** | ~32,736 reviews |
| **License** | Public dataset, no personally identifiable information (PII) |

---

## Access

The dataset is downloaded automatically via `kagglehub` on first run:

```python
import kagglehub
path = kagglehub.dataset_download("nelgiriyewithana/mcdonalds-store-reviews")
```

**Requirements:**
- A Kaggle account
- API credentials configured as one of:
  - `~/.kaggle/kaggle.json` (standard Kaggle CLI setup)
  - Environment variables `KAGGLE_USERNAME` and `KAGGLE_KEY`

The downloaded file is cached locally by `kagglehub` and reused on subsequent runs.

---

## Raw Columns

| Column | Type | Description |
|--------|------|-------------|
| `rating` | string | Star rating in the form `"1 star"` or `"4 stars"` |
| `review` | string | Free-text customer review |
| `store_address` | string | Full US address: `"street, city, state zip, country"` |
| `rating_count` | string | Total number of ratings for the store (may contain commas, e.g. `"1,234"`) |
| `store_name` | string | Store name — dropped during preprocessing |
| `category` | string | Category label — dropped during preprocessing |

---

## Preprocessing (`data_loader.py`)

All transformations are applied in `get_df()` and cached by Streamlit.

| Step | Input | Output | Detail |
|------|-------|--------|--------|
| Rating extraction | `"4 stars"` | `4` (Int64) | Regex `(\d+)` extracts the numeric value; column cast to nullable `Int64` |
| Address parsing | `"123 Main St, Springfield, IL 62701, United States"` | `street`, `city`, `zip`, `country` | Split on commas; ZIP extracted via `\b\d{5}(-\d{4})?\b` |
| Non-ASCII removal | raw review text | cleaned review text | `[^\x00-\x7F]+` stripped to avoid encoding issues |
| Rating count cleaning | `"1,234"` | `1234` (float) | Commas removed; converted via `pd.to_numeric` |
| `total_days_ago` | `years_ago`, `months_ago`, `days_ago`, `hours_ago` | float | `years×365 + months×30 + days + hours/24` |
| `review_length` | `review` | int | Character count via `str.len()` |
| Drop nulls | full DataFrame | clean DataFrame | `dropna()` removes any row with a missing value after all transforms |

Dropped columns: `store_name`, `category`, `store_address` (replaced by parsed components).

---

## Usage in the App

- **Review Dashboard (Tab 1):** filters by `city`; aggregates `rating` for KPI cards and charts.
- **AI Manager Co-Pilot (Tab 2):** filters by `street` (branch) and `total_days_ago`; sends up to 20 most recent `review` values to the Claude API.
- **Branch Health Overview:** groups by `street`, computes mean `rating` over the last 30 and 31–60 day windows for trend arrows.
