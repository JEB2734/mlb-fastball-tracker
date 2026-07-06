# mlb-fastball-tracker
# MLB Fastball Velocity Analysis (2015–2025)

**An end-to-end data pipeline that answers one question: is Major League Baseball throwing harder — and by how much?**

Using public Statcast pitch-tracking data, this project extracts raw pitch records with Python, stores and queries **386,162 pitches** in a cloud SQL database, and surfaces the findings through an interactive Tableau dashboard.

**🔗 [View the live interactive dashboard on Tableau Public →](https://public.tableau.com/app/profile/jared.brown4026/viz/MLBFastballVeloProject/FastballVelocityDashboard)**

![Dashboard preview](<img width="2818" height="1326" alt="image" src="https://github.com/user-attachments/assets/efa180a7-e40b-4333-bc76-d3532b13017e" />
)<img width="1419" height="667" alt="Screenshot 2026-07-05 at 8 06 19 PM" src="https://github.com/user-attachments/assets/6f71bf31-7beb-4944-94d0-cd01ba3a0b65" />

---

## Tech Stack

`Python` · `Microsoft SQL (Azure SQL)` · `SQL` · `Excel` · `Tableau` · `Git`

| Tool | Role in the project |
|------|---------------------|
| **Python** | Extract & clean Statcast data via `pybaseball`; generate batched SQL load files |
| **Azure SQL** | Cloud database storing all 386,162 pitch records; run analytical queries |
| **SQL** | Aggregations, conditional logic (`CASE WHEN`), filtering, `HAVING` clauses |
| **Excel** | Formula-driven summary workbook (deltas, spreads, totals) |
| **Tableau** | Interactive published dashboard — three coordinated views |
| **GitHub** | Version control for the extraction notebook and documentation |

---

## The Pipeline

```
Statcast  →  Python        →  Azure SQL      →  Excel          →  Tableau
(source)     (extract/clean)   (store/query)     (summarize)       (visualize)
```

1. **Extract** — A Python notebook pulls every four-seam fastball thrown during the **July 1–31 window** of each season, 2015–2025. Holding the calendar window constant keeps season-to-season comparisons fair.
2. **Load** — The 386,162-row dataset is loaded season-by-season into Azure SQL, with row counts verified after each load to guarantee data integrity.
3. **Analyze** — SQL queries compute the core trends: league-average velocity by season, pitching-staff rankings, and a hardest-throwers leaderboard.
4. **Summarize** — Results export to a formula-driven Excel workbook that recalculates deltas and spreads automatically.
5. **Visualize** — A published Tableau dashboard presents the season trend, team rankings, and pitcher leaderboard in one interactive view.

---

## Key Findings

### 1. Velocity is rising — and accelerating
League-average four-seam velocity climbed from **93.21 mph (2015)** to **94.59 mph (2025)** — a gain of **+1.38 mph** over the decade, with the steepest increases coming after 2021.

### 2. The hardest-throwing staffs are clearly separated
Attributing each pitch to the team that *threw* it (not the ballpark it was thrown in) reveals a **2.14 mph spread** between the top and bottom staffs — from the **NY Yankees at 95.14 mph** down to the **Chicago Cubs at 93.00 mph**.

### 3. A leaderboard of the game's hardest throwers
Filtering to pitchers with 100+ pitches produces a clean top-25 leaderboard led by **Mason Miller at 101.95 mph**. The recognizable names at the top served as a real-world sanity check on the pipeline.

### 4. A data-quality caveat worth flagging
Raw counts of 100+ mph pitches swing wildly year to year, but the *average velocity* of those pitches stays stable (~100.4–101.3 mph). This points to a change in Statcast's tracking technology rather than a real shift on the field — a reminder to interrogate surprising results before reporting them.

---

## Repository Structure

```
mlb-fastball-tracker/
├── pull_data.ipynb        # Python extraction & cleaning notebook
├── queries.sql            # SQL queries behind each finding
├── README.md              # You are here
└── dashboard-preview.png  # Dashboard screenshot (optional)
```

> **Note:** The raw ~386K-row CSV is intentionally **not** committed — it's large and fully regenerable by running `pull_data.ipynb`. Keeping generated data out of version control is standard practice.

---

## Reproducing This Project

```bash
# 1. Install dependencies
pip install pybaseball pandas

# 2. Run the extraction notebook to generate the dataset
#    (pull_data.ipynb pulls July four-seam fastballs, 2015–2025)

# 3. Load the data into your SQL database, then run queries.sql
```

---

## Data Source & Methodology

Data: **MLB Statcast**, accessed via the [`pybaseball`](https://github.com/jldbc/pybaseball) library.
Scope: four-seam fastballs (`pitch_type = 'FF'`), July 1–31 windows, 2015–2025. **n = 386,162 pitches.**
Team velocity is attributed via inning half (top/bottom) to credit the pitching team; the pitcher leaderboard is limited to pitchers with 100+ pitches.

*Built by Jared Brown — Management Information Systems, Florida State University.*
