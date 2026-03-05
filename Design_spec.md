# DESIGN_SPEC
## McDonald’s Feedback Analysis Social Media Command Centre

## 1. Scope and current implementation (source of truth)
This design spec is based on what the notebook currently implements:

### Data ingestion
- Pulls Kaggle dataset `nelgiriyewithana/mcdonalds-store-reviews`
- Loads `McDonald_s_Reviews.csv`

### Data preparation
- Drops `store_name` and `category`
- Drops null rows
- Cleans and splits `store_address` into:
  - `street`, `city`, `zip`, `country`
- Parses `review_time` into:
  - `years_ago`, `months_ago`, `days_ago`, `hours_ago`
  - `total_days_ago`
- Cleans `rating_count` (removes commas, casts to integer)
- Adds `review_length`

### Analysis outputs (visuals and summaries)
- Rating distribution chart
- City and street level rating summaries (mean, count)
- Scatter plots (geo and review length relationships)
- Correlation heatmap for numeric fields
- Folium map of store locations (based on `latitude ` and `longitude`)

---

## 2. Product goal (MVP)
Provide a command centre style interface that helps management answer:
1. Where are rating issues concentrated geographically?
2. Which cities and store streets have the lowest ratings and highest review volumes?
3. How do ratings relate to review length, rating count, and recency?
4. What has changed recently versus older reviews using `total_days_ago`?

---

## 3. Users and needs
### Primary user: Regional Operations Analyst
- Needs city level and store location comparisons.
- Needs quick identification of low performing locations.
- Needs exports and visuals for weekly reporting.

### Secondary user: Store Manager
- Needs a simple view of their city or street performance.
- Needs recent feedback focus (recency filtering).

### Supporting user: Data analyst
- Validates data quality and feature engineering outputs.

---

## 4. User journeys

### Journey A: Identify low rating hotspots (Ops Analyst)
1. Open the command centre.
2. Filter to a region (country, city) and set a recency window.
3. Review the map and city leaderboard to find low average ratings with high volume.
4. Drill into a specific street or city.
5. Export a summary table and screenshots for reporting.

### Journey B: Understand what correlates with low ratings (Ops Analyst)
1. Open analytics view.
2. Compare rating distribution with `review_length`, `rating_count`, and `total_days_ago`.
3. Use correlation heatmap to prioritize which signals to investigate.
4. Save insights and share with stakeholders.

### Journey C: Validate preprocessing quality (Data analyst)
1. Open data quality view.
2. Confirm address parsing coverage and review time parsing coverage.
3. Spot check outliers (missing zip, unusual coordinates, inconsistent types).
4. Document issues and adjustments needed.

---

## 5. Task flows

### Flow 1: Data refresh and preprocessing (system flow)
1. Download dataset from Kaggle.
2. Load CSV to dataframe.
3. Drop unused columns and null rows.
4. Parse address into `street`, `city`, `zip`, `country`.
5. Parse time into `years_ago`, `months_ago`, `days_ago`, `hours_ago`.
6. Compute `total_days_ago`.
7. Clean `rating_count` and compute `review_length`.

### Flow 2: Explore performance by geography (user flow)
1. Select filters:
   - Country, city, zip
   - Rating range
   - Recency window using `total_days_ago`
2. View map markers and city leaderboard.
3. Drill into a city, then a street.
4. View rating distribution and review volume for selection.
5. Export filtered table.

### Flow 3: Explore relationships and drivers (user flow)
1. Open analytics view.
2. Review:
   - Rating distribution
   - Geo scatter plot colored by rating
   - Review length versus rating scatter plot
   - Correlation heatmap
3. Apply filters and compare results.
4. Save results for reporting.

---

## 6. Key screens and interactions
The “screens” below describe the UI the team should build to expose the notebook outputs in a command centre workflow.

### Screen 1: Overview
**Purpose:** One place to understand dataset scope and current performance.

**Components**
- KPI tiles: total reviews, average rating, top cities by volume, lowest cities by rating
- Rating distribution chart
- Quick filters: country, city, recency window

**Key interactions**
- Click a city to apply filter across all screens
- Adjust recency window to focus on recent experience

---

### Screen 2: Geography Command Centre
**Purpose:** Identify hotspots and drill down by location.

**Components**
- Interactive map (Folium) using `latitude ` and `longitude`
- City leaderboard table:
  - average rating by city
  - review count by city
- Street leaderboard table for selected city:
  - mean rating and count by street (based on `street` aggregation)

**Key interactions**
- Click map marker to open a location panel:
  - street, city, zip, country
  - average rating and count for that street
- Sort leaderboards by mean rating, then by count
- Filter to “high volume and low rating” using thresholds

---

### Screen 3: Ratings and Volume Analytics
**Purpose:** Understand distribution and skew.

**Components**
- Rating distribution bar chart
- City rating distribution (optional)
- Review volume distribution by city

**Key interactions**
- Filter by rating range (example: show only 1 to 2 star)
- Compare selected city versus overall baseline

---

### Screen 4: Recency and Trend Lens
**Purpose:** Separate recent experience from historical feedback.

**Components**
- Recency slider for `total_days_ago`
- Summary cards for selected recency window:
  - average rating
  - review count
  - top cities by count in the window

**Key interactions**
- Toggle between “last X days” versus “all time”
- Use recency filter to refresh geography and analytics screens

---

### Screen 5: Correlations and Diagnostics
**Purpose:** Support hypothesis generation and data validation.

**Components**
- Correlation heatmap across:
  - `rating`, `rating_count`, `review_length`, `total_days_ago`
- Scatter plot:
  - review_length versus rating
  - optional coloring by rating_count buckets

**Key interactions**
- Click a heatmap cell to highlight the related chart
- Export the correlation table and filtered dataset for deeper analysis

---

### Screen 6: Data Explorer
**Purpose:** Audit records and support spot checks.

**Components**
- Table with essential columns:
  - `review`, `rating`, `rating_count`, `review_time`
  - `street`, `city`, `zip`, `country`
  - `latitude `, `longitude`
  - `review_length`, `total_days_ago`

**Key interactions**
- Search in review text
- Export filtered rows to CSV
- Quick outlier filters (missing zip, extreme coordinates, very long reviews)

---

## 7. Data fields required by the UI
Minimum required fields based on the notebook:
- `review` (text)
- `rating` (integer)
- `rating_count` (integer)
- `review_time` (string)
- `street`, `city`, `zip`, `country`
- `latitude ` and `longitude` (note: latitude column has a trailing space in the dataset)
- Derived:
  - `years_ago`, `months_ago`, `days_ago`, `hours_ago`
  - `total_days_ago`
  - `review_length`

---

## 8. Demo of critical flows (MVP)
This demo plan is designed to show the most important end to end value using the current dataset and outputs.

### Demo Flow 1: Find a low rating hotspot in Chicago
1. Open Geography Command Centre.
2. Filter country to United States.
3. Filter city to Chicago (or nearby cities if dataset coverage differs).
4. Sort city leaderboard by lowest average rating.
5. Click a low performing city entry to filter map and street leaderboard.
6. Identify a street with low mean rating and high count.
7. Export the street summary and capture map screenshot.

**Success criteria**
- A reviewer can see a concrete location with measurable low performance.

### Demo Flow 2: Show recency impact
1. Open Recency and Trend Lens.
2. Set recency to the last 30 days using `total_days_ago`.
3. Compare average rating and city leaderboard versus all time.
4. Capture before and after screenshots.

**Success criteria**
- A reviewer can see that recent feedback differs from historical feedback and can isolate the change.

### Demo Flow 3: Explain a hypothesis with analytics
1. Open Correlations and Diagnostics.
2. Show correlation heatmap and interpret one relationship.
3. Open scatter plot review_length versus rating and explain what it suggests.
4. Export filtered dataset for follow up.

**Success criteria**
- A reviewer can see evidence backed reasoning, not only charts.

---

## 9. Success metrics (MVP)
- Time to identify bottom 5 cities by average rating decreases.
- Ability to isolate recent low ratings using `total_days_ago`.
- Data quality checks pass:
  - address parsing coverage is high
  - time parsing coverage is high
- Analysts can export summaries without manual dataframe edits.

---

## 10. Known gaps and planned extensions (not implemented yet)
- Operational issue extraction from review text (wait time, order accuracy, cleanliness).
- Aspect level sentiment and sarcasm handling.
- Rating text mismatch detection.
- Uncertainty handling and an “ambiguous queue”.
- Action tracking workflow (assign, resolve, measure impact).

These are valid next steps, but the MVP UI and demo flows above are intentionally scoped to match the current notebook outputs.

---
