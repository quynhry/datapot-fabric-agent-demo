# Report spec — VanArsdel Executive Dashboard

## Purpose & audience

- **Audience:** CEO, VP Sales, Regional Manager, Marketing Manager
- **Decisions supported:** Theo dõi hiệu suất doanh thu vs. ngân sách ở giai đoạn đủ dữ liệu; xác định vùng/sản phẩm/kênh cần hành động
- **Refresh / latency:** Manual refresh; Import mode

---

## Key questions

1. Doanh thu YTD so với Budget đang ở mức nào trong giai đoạn có overlap dữ liệu?
2. Segment / Category / Product nào over/under-performing?
3. Khu vực địa lý nào có hiệu suất tốt nhất / kém nhất?
4. Xu hướng doanh thu qua các năm (2015–2020) và tính mùa vụ?
5. Tổ hợp Traffic Channel x Device nào tương quan với tăng doanh thu?
6. Chỉ số nào cần gắn nhãn "không đủ dữ liệu" để tránh hiểu nhầm (Forecast variance, kỳ ngoài overlap)?

---

## KPIs

| KPI | Measure (dự kiến) | Target / benchmark |
|-----|-------------------|--------------------|
| Total Revenue | `[Total Revenue]` | Budget Amount |
| Total Cost | `[Total Cost]` | — |
| Gross Profit | `[Gross Profit]` | — |
| Gross Margin % | `[Gross Margin %]` | — |
| Budget Attainment % | `[Budget Attainment %]` | ≥ 100% |
| Revenue Variance vs Budget | `[Variance vs Budget]` | ≥ 0 |
| Revenue YoY % | `[Revenue YoY %]` | > 0% |
| Revenue YTD | `[Revenue YTD]` | Budget YTD |
| Total Units Sold | `[Total Units]` | Volume plan |

---

## Grain & dimensions

- **Fact grain (Sales):** ngày × khách hàng × sản phẩm × địa điểm × (campaign)
- **Fact grain (Budget):** tháng × Segment-Category × Type (Budget / Forecast)
- **Dimensions / slicers:**
  - Date: Year, Quarter, Month
  - Product: Category → Segment → Product
  - Geo: Country → State → City
  - Campaign: Traffic Channel, Device
  - Type filter: Budget vs. Forecast

---

## Pages

### Page 1 — Overview (Executive Summary)

- **Purpose:** Bức tranh tổng quan — Revenue vs. Budget, trend, và top contributors
- **Audience:** CEO / C-level
- **Visuals:**
  - KPI cards (top row): Total Revenue YTD · Budget Attainment % · Variance vs Budget · Revenue YoY %
  - Line chart: Monthly Revenue trend (Actual vs Budget) — X: Month, Y: Revenue, lines: Actual / Budget
  - Bar chart: Revenue by Segment — horizontal bar, sorted desc, color-coded vs. budget
  - Map: Revenue by Country/State — bubble size = Revenue, color = Budget Attainment %
- **Slicers (page-level):** Year (default: latest), Quarter
- **Data note:** Hiển thị badge "Insufficient overlap" ngoài giai đoạn Jan-Jun 2020 khi user chọn visual so sánh Actual vs Budget.

---

### Page 2 — Product Performance

- **Purpose:** Drill-down vào hiệu suất theo phân cấp sản phẩm
- **Audience:** VP Sales, Product team
- **Visuals:**
  - KPI cards: Total Revenue · Total Units · Top Segment (by Revenue)
  - Matrix (drilldown): Category → Segment → Product | Columns: Revenue / Units / Budget / Attainment % / YoY %
  - Waterfall chart: Revenue contribution by Segment — hiển thị contribution và variance
  - Scatter plot: Category performance — X: Revenue, Y: YoY %, size: Units — để identify "stars" vs. "laggards"
- **Slicers:** Year, Category, Segment
- **Data note:** `Rural-Productivity` thiếu cột budget; visual so sánh budget cần chú thích rõ logic xử lý.

---

### Page 3 — Geographic Performance

- **Purpose:** Phân tích theo vùng địa lý
- **Audience:** Regional Manager, VP Sales
- **Visuals:**
  - Filled map: Revenue by State — color = Budget Attainment % (green/amber/red)
  - Bar chart: Top 10 Cities by Revenue
  - Table: State summary — State / Revenue / Budget / Attainment % / YoY % / Rank
  - KPI card: Best performing region · Worst performing region
- **Slicers:** Year, Country, Segment

---

### Page 4 — Trend & Seasonality

- **Purpose:** Phân tích xu hướng dài hạn và tính mùa vụ
- **Audience:** C-level, Finance
- **Visuals:**
  - Line chart (multi-year overlay): Revenue by Month, mỗi line = 1 năm (2015–2020) — nhìn thấy seasonality
  - Area chart: Revenue YTD cumulative by year
  - Column chart: Monthly YoY % Growth — conditional formatting (positive/negative)
  - Small multiples (nếu hỗ trợ): Revenue by Segment qua từng năm
- **Slicers:** Segment, Category, Country
- **Data note:** Nếu Date dimension chưa mở rộng về 2015, phải ẩn năm 2015 khỏi tất cả visual theo thời gian và ghi chú phạm vi báo cáo.

---

### Page 5 — Traffic Channel Performance

- **Purpose:** Đánh giá hiệu suất nguồn lưu lượng và thiết bị
- **Audience:** Marketing, Sales Director
- **Visuals:**
  - Bar chart: Revenue by Traffic Channel (top to bottom)
  - Stacked column: Revenue by Traffic Channel x Device
  - Trend line: Monthly Revenue by selected channel/device
  - KPI cards: Top Channel · Top Device · Share of top channel
- **Slicers:** Year, Category, Segment, Device

---

### Page 6 — Campaign Analysis

- **Purpose:** Data quality & mapping view cho Campaign dimension
- **Audience:** Marketing Manager
- **Visuals:**
  - Table: CampaignID / TrafficChannel (raw) / TrafficChannel (clean) / Device (raw) / Device (clean)
  - KPI card: Số bản ghi cần chuẩn hóa (trim + typo)
  - Bar chart: Revenue impact before/after standardization group
- **Slicers:** Year, Traffic Channel, Device

---

### Page 7 — Detail (Drill-through)

- **Purpose:** Bảng chi tiết giao dịch — drill-through từ các page khác
- **Audience:** Analyst / Regional Manager
- **Visuals:**
  - Table: Date / Product / Category / Segment / City / Revenue / Units / Manufacturer
  - Back button (drill-through return)
- **Slicers:** Kế thừa context từ trang gốc (drill-through filters)
- **Note:** Không hiển thị trong nav bar mặc định — chỉ accessible qua right-click → drill-through

---

## Filters

- **Report-level:** Exclude unknown/invalid keys sau khi ETL chuẩn hóa
- **Page-level:** Xem từng page ở trên
- **Cross-report drill-through:** Detail page nhận filters từ mọi page

---

## Design notes

- **Theme:** `shared/themes/datapot-theme.json`. Page size: 1280 × 720 px. Format via theme.
- **Color encoding:** Budget Attainment: `≥ 100%` = green, `80–99%` = amber, `< 80%` = red.
- **Navigation:** Tab bar ở đầu mỗi page với icon + tên page.
- **Measure table:** Tất cả measures đặt trong `Key Measures` table, ẩn khỏi các bảng fact/dim.
- **Fact columns:** Ẩn tất cả cột fact (Revenue, Units, keys) — chỉ expose qua measures.
- **Tooltip:** KPI cards có tooltip giải thích định nghĩa metric.
- **Language:** Labels và tooltips bằng tiếng Anh.
