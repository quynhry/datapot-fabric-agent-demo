# Wireframe / Mockup — VanArsdel Executive Dashboard

Layout phác thảo cho từng page. Mỗi visual được chú thích metric và chiều dữ liệu hiển thị.
Kích thước page: 1280 × 720 px (16:9).

---

## Page 1 — Overview (Executive Summary)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [Nav: Overview | Product | Geography | Trend | Market Share | Campaign | Detail]   │
│  Slicers: [Year ▾] [Quarter ▾]                                             [Logo]  │
├───────────────┬───────────────┬───────────────┬───────────────────────────────────┤
│  KPI CARD     │  KPI CARD     │  KPI CARD     │  KPI CARD                          │
│  Total Rev YTD│  Attainment % │  Variance $   │  YoY Growth %                      │
│  $12.5M       │  94.3%        │  -$740K       │  +8.2%                             │
│  ▲ vs Budget  │  🟡 amber     │  ↓ bad        │  ↑ good                            │
├───────────────┴───────────────┴───────────────┴───────────────────────────────────┤
│                                                                                     │
│  LINE CHART: Monthly Revenue — Actual vs Budget vs Forecast                        │
│  X: Month (Jan–Dec)   Y: Revenue ($)                                               │
│  Lines: ── Actual  ┄┄ Budget  ··· Forecast                                         │
│  (full width, ~350px tall)                                                          │
│                                                                                     │
├────────────────────────────────────┬────────────────────────────────────────────────┤
│  HORIZ BAR: Revenue by Segment     │  MAP: Revenue by State                         │
│  Segment A  ████████████  $5.2M   │  (bubble map or filled choropleth)             │
│  Segment B  ██████████    $4.1M   │  Bubble size = Revenue                         │
│  Segment C  ██████        $3.2M   │  Color = Budget Attainment %                   │
│  (sorted desc, conditional color) │  (green=≥100%, amber=80-99%, red=<80%)         │
└────────────────────────────────────┴────────────────────────────────────────────────┘
```

---

## Page 2 — Product Performance

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [Nav bar]                                          Slicers: [Year ▾] [Mfr ▾]      │
├──────────────────┬──────────────────┬──────────────────────────────────────────────┤
│  KPI: Revenue    │  KPI: Units      │  KPI: Top Segment (by Revenue)               │
│  $12.5M          │  425K units      │  Urban · $5.2M                               │
├──────────────────┴──────────────────┴──────────────────────────────────────────────┤
│  MATRIX (drilldown: Segment → Category → Product)                                   │
│  +--------------+----------+--------+----------+------------+--------+             │
│  | Name         | Revenue  | Units  | Budget   | Attain %   | YoY %  |             │
│  | ▶ Urban      | $5.2M    | 180K   | $5.5M    | 94.5%      | +9%    |             │
│  |   Extreme    | $2.1M    | 70K    | $2.2M    | 95.5%      | +12%   |             │
│  |   Moderate   | $1.8M    | 60K    | $1.9M    | 94.7%      | +7%    |             │
│  | ▶ Rural      | $4.1M    | 140K   | $4.0M    | 102.5%     | +6%    |             │
│  +--------------+----------+--------+----------+------------+--------+             │
├────────────────────────────┬────────────────────────────────────────────────────────┤
│  WATERFALL: Revenue        │  SCATTER: Category performance                         │
│  Contribution by Segment   │  X: Revenue  Y: YoY %  Size: Units                    │
│  (shows + and - segments)  │  Each dot = 1 Category; label on hover                │
│                            │  Quadrants: Star / Cash Cow / Question / Dog          │
└────────────────────────────┴────────────────────────────────────────────────────────┘
```

---

## Page 3 — Geographic Performance

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [Nav bar]                              Slicers: [Year ▾] [Country ▾] [Segment ▾]  │
├─────────────────────────────────────────┬───────────────────────────────────────────┤
│  FILLED MAP: Revenue by State           │  BAR: Top 10 Cities by Revenue            │
│  Color = Budget Attainment %            │  Horizontal bars, sorted desc             │
│  (green/amber/red)                      │  Each bar = City name + $ revenue         │
│  Tooltip: State, Revenue, Attain%       │                                           │
│                                         │                                           │
│                                         ├───────────────────────────────────────────┤
│                                         │  KPI CARDS (2x)                           │
│                                         │  Best Region · Worst Region               │
│                                         │  (dynamic text based on filter)           │
├─────────────────────────────────────────┴───────────────────────────────────────────┤
│  TABLE: State Summary                                                               │
│  State | Revenue | Budget | Attain % | YoY % | Rank                                │
│  (conditional formatting on Attain % column; sortable)                              │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Page 4 — Trend & Seasonality

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [Nav bar]                              Slicers: [Segment ▾] [Category ▾] [Country ▾]│
├─────────────────────────────────────────────────────────────────────────────────────┤
│  LINE CHART: Monthly Revenue by Year (2015–2020 overlay)                            │
│  X: Month (1–12)   Y: Revenue   Each line = 1 year (different color)               │
│  → Reveals seasonality pattern clearly                                              │
│  (full width, tall ~300px)                                                          │
├────────────────────────────────────────┬────────────────────────────────────────────┤
│  AREA CHART: YTD Cumulative Revenue    │  COLUMN CHART: Monthly YoY Growth %        │
│  X: Month  Y: Cumulative Revenue       │  X: Month  Y: YoY %                       │
│  Lines = each year stacked area        │  Color: green if ≥ 0, red if < 0          │
│  (shows acceleration/deceleration)     │  Reference line at 0%                     │
└────────────────────────────────────────┴────────────────────────────────────────────┘
```

---

## Page 5 — Market Share

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [Nav bar]                     Slicers: [Year ▾] [Segment ▾] [Category ▾] [Country ▾]│
├──────────────────────┬─────────────────────────────────────────────────────────────┤
│  DONUT CHART         │  LINE CHART: VanArsdel Market Share % over time             │
│  Market Share by     │  X: Month  Y: Market Share %                                │
│  Manufacturer        │  Reference line: average market share                       │
│                      │                                                              │
│  VanArsdel: 34%  ●  │                                                              │
│  Competitor A: 22% ● │                                                              │
│  Others: 44%      ● │                                                              │
├──────────────────────┴──────────────────┬───────────────────────────────────────────┤
│  KPI CARDS                              │  STACKED BAR: Revenue by Manufacturer     │
│  VanArsdel Share %  ·  Rank vs Peers   │  × Segment                               │
│                                         │  X: Segment  Y: Revenue                  │
│                                         │  Color = Manufacturer                    │
└─────────────────────────────────────────┴───────────────────────────────────────────┘
```

---

## Page 6 — Campaign Analysis

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [Nav bar]                         Slicers: [Year ▾] [Campaign Type ▾] [Segment ▾]  │
├──────────────────────────────────────────────────────────────────────────────────────┤
│  REVENUE + CAMPAIGN TIMELINE COMBO CHART                                            │
│  Primary Y: Revenue (line/area)  X: Month                                          │
│  Overlay: Campaign period as colored band / annotation                              │
│  → DA/analyst can visually see if revenue spiked during campaigns                   │
├────────────────────────────────────────┬────────────────────────────────────────────┤
│  BAR CHART: Revenue by Campaign        │  TABLE: Campaign Detail                    │
│  Top 10 campaigns, sorted by Revenue  │  Name · Type · Start · End · Rev · Lift %  │
│  Color = Campaign Type                 │  (sortable; conditional formatting on Lift) │
│                                        │                                            │
│  KPI CARDS (below bars):              │                                            │
│  Total Campaigns · Avg Lift %          │                                            │
└────────────────────────────────────────┴────────────────────────────────────────────┘
```

---

## Page 7 — Detail (Drill-through)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│  [← Back]   Detail View                                                             │
│  Context filters inherited from drill-through (shown as pills)                      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  TABLE: Transaction detail                                                          │
│  Date | Product | Category | Segment | Manufacturer | City | State | Revenue | Units│
│  (paginated; sortable; search)                                                      │
│                                                                                     │
│  Total row at bottom: SUM Revenue · SUM Units                                       │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Navigation structure

```
Page 1 Overview  →  drill-through  →  Page 7 Detail
Page 2 Product   →  drill-through  →  Page 7 Detail
Page 3 Geography →  drill-through  →  Page 7 Detail
(Pages 4, 5, 6: no drill-through — aggregate only)
```

## Color palette (from datapot-theme.json)

| Usage | Color |
|-------|-------|
| Actual / primary metric | Theme primary (blue) |
| Budget | Neutral gray |
| Forecast | Dashed teal |
| Positive / ≥ 100% | Green |
| Warning / 80–99% | Amber |
| Negative / < 80% | Red |
| VanArsdel highlight | Theme accent |
