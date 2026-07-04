# Data dictionary — VanArsdel Executive Dashboard



## Conventions

- **Model Name** = tên thân thiện trong semantic model (Title Case). **Source Column** = tên cột vật lý trong file nguồn.
- Types: `int64`, `double`, `string`, `date`, `dateTime`, `boolean`.
- Key: `PK` = primary key, `FK` = foreign key, `–` = attribute.

---

## Quan hệ giữa các bảng (Relationships — đã xác thực referential integrity)

```
Sales.ProductID   ──n:1──> Product.ProductID
Sales.Date        ──n:1──> Date.Date
Sales.CustomerID  ──n:1──> Customer.CustomerID
Sales.CampaignID  ──n:1──> Campaign.CampaignID
Customer.ZipCode  ──n:1──> Geo.Zip
Budget (Category, Segment) ──n:1──> Product (Category, Segment)  [lookup join, không phải FK cột đơn]
```

> ⚠️ **Date gap:** Sales có data từ `2015-01-01` nhưng Date dimension chỉ bắt đầu từ `2016-01-01`.
> Các giao dịch năm 2015 (~1 năm đầu) **sẽ không join được** với Date dimension.
> Cần quyết định: (a) mở rộng Date dimension về 2015, hoặc (b) ghi chú phạm vi có thể drill-down = 2016–2020.
> Xem thêm `data-gap-questions.md`.

---

## Table: `Date`  *(dimension)*

- **Grain:** 1 dòng = 1 ngày dương lịch
- **Phủ:** `2016-01-01` → `2021-12-31` (không có năm 2015)
- **Source file:** `dataset/raw/date.csv` (sheet `Date`)

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Date | `Date` | dateTime | PK | no | Ngày dương lịch | `2021-01-01` |
| Month No | `MonthNo` | int64 | – | no | Số thứ tự tháng (1–12) | `1` |
| Month Name | `MonthName` | string | – | no | Tên tháng viết tắt | `Jan` |
| Month ID | `MonthID` | int64 | – | no | Mã tháng-năm dạng YYYYMM | `202101` |
| Month | `Month` | string | – | no | Mã ngắn tháng-năm | `Jan-21` |
| Quarter | `Quarter` | string | – | no | Quý trong năm | `Q1` |
| Year | `Year` | int64 | – | no | Năm | `2021` |

---

## Table: `Campaign`  *(dimension)*

- **Grain:** 1 dòng = 1 tổ hợp kênh × thiết bị (22 dòng)
- **Source file:** `dataset/raw/campaign.csv` (sheet `Campaign`)
- **Lưu ý:** "Campaign" trong dataset này **không phải chiến dịch marketing có tên**, mà là tổ hợp **Traffic Channel × Device** — tức là nguồn dẫn đến giao dịch (giống UTM source/medium). Đây là điểm khác biệt quan trọng với định nghĩa Campaign thông thường.

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Campaign ID | `CampaignID` | int64 | PK | no | ID tổ hợp kênh/thiết bị | `1` |
| Traffic Channel | `TrafficChannel` | string | – | no | Kênh lưu lượng | `Organic Search` |
| Device | `Device` | string | – | no | Thiết bị | `Desktop` |

---

## Table: `Customer`  *(dimension)*

- **Grain:** 1 dòng = 1 khách hàng (282.597 dòng)
- **Source file:** `dataset/raw/customer.csv` (sheet `Customer`)

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Customer ID | `CustomerID` | int64 | PK | yes | ID khách hàng | `1` |
| Zip Code | `ZipCode` | int64 | FK → Geo.Zip | yes | Mã bưu chính, dùng để nối sang Geo | `90250` |
| Email Name | `Email Name` | string | – | no | Chuỗi ghép email + họ tên dạng `(email): Họ, Tên` | `(Meghan.Alexander@xyza.com): Alexander, Meghan` |

---

## Table: `Geo`  *(dimension)*

- **Grain:** 1 dòng = 1 mã bưu chính (39.948 dòng)
- **Source file:** `dataset/raw/geo.csv` (sheet `Geo`)

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Zip | `Zip` | int64 | PK | no | Mã bưu chính | `22654` |
| City | `City` | string | – | no | Thành phố — chuỗi đã gồm bang & quốc gia | `Star Tannery, VA, USA` |
| State | `State` | string | – | no | Mã bang 2 ký tự | `VA` |
| Region | `Region` | string | – | no | Vùng địa lý lớn | `East` |
| District | `District` | string | – | no | Khu vực nhỏ hơn Region | `District #07` |
| Country | `Country` | string | – | no | Quốc gia | `USA` |

---

## Table: `Product`  *(dimension)*

- **Grain:** 1 dòng = 1 sản phẩm (212 dòng)
- **Source file:** `dataset/raw/product.csv` (sheet `Product`)
- **Phân cấp:** `Category` (5 giá trị) → `Segment` (9 giá trị) → `Product` (212 sản phẩm)

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Product ID | `ProductID` | int64 | PK | yes | ID sản phẩm | `392` |
| Product | `Product` | string | – | no | Tên sản phẩm | `Maximus RP-01` |
| Category | `Category` | string | – | no | Nhóm sản phẩm cấp cao (5 giá trị) | `Rural` |
| Segment | `Segment` | string | – | no | Phân lớp chi tiết trong Category (9 giá trị) | `Productivity` |
| Manufacturer ID | `ManufacturerID` | int64 | – | yes | Mã nhà sản xuất | `7` |
| Manufacturer | `Manufacturer` | string | – | no | Tên nhà sản xuất — `VanArsdel` hoặc đối thủ | `VanArsdel` |
| Unit Cost | `Unit Cost` | double | – | yes | Giá vốn 1 đơn vị, cố định theo ProductID | `37.27` |
| Unit Price | `Unit Price` | double | – | yes | Giá bán 1 đơn vị, cố định theo ProductID | `51.06` |

---

## Table: `Sales`  *(fact)*

- **Grain:** 1 dòng = 1 đơn vị sản phẩm được bán (`Units` luôn = 1 trên toàn bộ dataset)
- **Dòng:** 675.368 dòng, phủ `2015-01-01` → `2020-06-30`
- **Source file:** `dataset/raw/sales.csv` (sheet `Sales`)
- **Lưu ý:**
  - Không có cột ID/PK riêng; tổ hợp `(ProductID, Date, CustomerID, CampaignID)` có thể dùng làm surrogate key.
  - **Không có cột Revenue** — phải tính `Revenue = Units × Unit Price` (lấy Unit Price từ Product dimension).
  - Units luôn = 1, nên Revenue per row = Unit Price của sản phẩm đó.

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Product ID | `ProductID` | int64 | FK → Product | yes | Sản phẩm được bán | `676` |
| Date | `Date` | dateTime | FK → Date.Date | yes | Ngày bán | `2015-09-25` |
| Customer ID | `CustomerID` | int64 | FK → Customer | yes | Khách mua | `70283` |
| Campaign ID | `CampaignID` | int64 | FK → Campaign | yes | Nguồn/kênh dẫn đến giao dịch | `22` |
| Units | `Units` | int64 | – | yes | Luôn = 1 trên toàn bộ dữ liệu | `1` |

---

## Table: `Budget / Forecast`  *(fact — wide format, cần unpivot)*

- **Grain gốc (wide):** 1 dòng = 1 tổ hợp (Type, Year, Month); 9 cột giá trị theo Category-Segment
- **Grain sau unpivot:** 1 dòng = 1 tổ hợp (Type, Year, Month, Category, Segment)
- **Source file:** `dataset/raw/budget_forecast.csv`
- **Join với Sales:** qua `Category` + `Segment` (từ Product dim) + `Year` + `Month`

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Type | `Type` | string | – | no | `Budget` hoặc `Forecast` | `Budget` |
| Year | `Year` | int64 | – | no | Năm | `2020` |
| Month | `Month` | string | – | no | Tháng viết tắt 3 ký tự | `Jun` |
| Category | *(tách từ tên cột, trước dấu `-`)* | string | – | no | Khớp với `Product.Category` | `Urban` |
| Segment | *(tách từ tên cột, sau dấu `-`)* | string | – | no | Khớp với `Product.Segment` | `Convenience` |
| Value | *(giá trị ô tương ứng cột Category-Segment)* | double | – | no | Doanh thu mục tiêu/dự báo của tháng cho tổ hợp Category-Segment | `44650.90` |

---

## Measures  *(all on `Key Measures`)*

| Measure | Folder | Format | DAX (gợi ý) | Definition |
|---------|--------|--------|-------------|------------|
| Total Revenue | 01 Revenue | `#,0.00` | `SUMX(Sales, Sales[Units] * RELATED(Product[Unit Price]))` | Tổng doanh thu thực tế = Units × Unit Price |
| Total Units | 01 Revenue | `#,0` | `SUM(Sales[Units])` | Tổng số lượng bán (= số dòng Sales vì Units luôn = 1) |
| Total Cost | 01 Revenue | `#,0.00` | `SUMX(Sales, Sales[Units] * RELATED(Product[Unit Cost]))` | Tổng giá vốn hàng bán |
| Gross Profit | 01 Revenue | `#,0.00` | `[Total Revenue] - [Total Cost]` | Lợi nhuận gộp |
| Gross Margin % | 01 Revenue | `0.0%` | `DIVIDE([Gross Profit], [Total Revenue])` | Biên lợi nhuận gộp |
| Budget Amount | 02 Budget | `#,0.00` | `CALCULATE(SUM(Budget[Value]), Budget[Type]="Budget")` | Tổng ngân sách kỳ |
| Forecast Amount | 02 Budget | `#,0.00` | `CALCULATE(SUM(Budget[Value]), Budget[Type]="Forecast")` | Tổng forecast kỳ |
| Budget Attainment % | 02 Budget | `0.0%` | `DIVIDE([Total Revenue], [Budget Amount])` | Tỷ lệ hoàn thành ngân sách |
| Variance vs Budget | 02 Budget | `#,0.00` | `[Total Revenue] - [Budget Amount]` | Chênh lệch tuyệt đối thực tế − ngân sách |
| Variance % vs Budget | 02 Budget | `+0.0%;-0.0%` | `DIVIDE([Variance vs Budget], [Budget Amount])` | Chênh lệch tương đối vs ngân sách |
| Revenue YoY % | 03 Growth | `+0.0%;-0.0%` | `DIVIDE([Total Revenue] - [Revenue PY], [Revenue PY])` | Tăng trưởng doanh thu so cùng kỳ năm ngoái |
| Revenue PY | 03 Growth | `#,0.00` | `CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Date[Date]))` | Doanh thu cùng kỳ năm ngoái |
| Revenue YTD | 03 Growth | `#,0.00` | `TOTALYTD([Total Revenue], Date[Date])` | Doanh thu lũy kế từ đầu năm |

> **Lưu ý phạm vi:** Manufacturer hiện là hằng số `VanArsdel` trong toàn bộ Product, nên các measure thị phần/so sánh đối thủ không nằm trong scope phiên bản dashboard này.

> **Lưu ý quan trọng:** `Total Revenue` **không dùng** `SUM` trực tiếp mà phải dùng `SUMX` với `RELATED(Product[Unit Price])` vì Sales không có cột Revenue sẵn.
