# Business glossary - VanArsdel

Định nghĩa các khái niệm nghiệp vụ dùng xuyên suốt bộ tài liệu.
Chi tiết kỹ thuật cột/kiểu/khóa xem `data-dictionary.md`.

---

## 1.1 Segment vs Category - quan hệ phân cấp

Trong dữ liệu VanArsdel, `Category` là cấp cao hơn `Segment`.

- `Category` (5 giá trị): `Accessory`, `Mix`, `Rural`, `Urban`, `Youth`
- `Segment` (9 giá trị): `Accessory`, `All Season`, `Convenience`, `Extreme`, `Moderation`, `Productivity`, `Regular`, `Select`, `Youth`
- Cột ngân sách đặt theo quy ước `Category-Segment` (ví dụ `Urban-Convenience`)

| Category | Segment | Số Product |
|----------|---------|------------|
| Accessory | Accessory | 10 |
| Mix | All Season | 10 |
| Mix | Productivity | 10 |
| Rural | Productivity | 2 |
| Rural | Select | 1 |
| Urban | Convenience | 79 |
| Urban | Extreme | 17 |
| Urban | Moderation | 72 |
| Urban | Regular | 1 |
| Youth | Youth | 10 |

Tổng cộng: **212 products**.

---

## 1.2 Campaign - kênh và thiết bị

`Campaign` trong bộ dữ liệu này không phải chiến dịch marketing theo nghĩa tên/ngân sách/thời gian chạy.
Mỗi `CampaignID` là một tổ hợp cố định `TrafficChannel x Device`.

- 22 `CampaignID` tương ứng 8 `TrafficChannel` với 2-3 `Device` mỗi kênh
- `TrafficChannel`: `Organic Search`, `SEO`, `Banner`, `Affliliate`, `SEM`, `Email`, `SMO`, `Mail`
- `Device`: `Desktop`, `Mobile`, `Tablet`; riêng kênh `Mail` dùng `Paper`

Lưu ý ETL: cần `TRIM` và sửa lỗi chính tả (`Affliliate`, `Deskop`) trước khi hiển thị dashboard.

---

## 1.3 Manufacturer vs Product

Toàn bộ 212 sản phẩm trong `Product` đều có `Manufacturer = VanArsdel`.
Hiện không có dữ liệu đối thủ trong dataset.

Hệ quả nghiệp vụ:
- Không có ý nghĩa phân tích thị phần/so sánh cạnh tranh theo Manufacturer trong phiên bản dữ liệu này
- `ManufacturerID` vẫn nên giữ trong model để sẵn sàng mở rộng dữ liệu tương lai

---

## 1.4 Đơn vị doanh thu và công thức

Không có cột `Revenue`/`Cost`/`Profit` sẵn trong nguồn, cần tính:

- `Revenue = Units x Unit Price`
- `Cost = Units x Unit Cost`
- `Profit = Revenue - Cost`

Lưu ý dữ liệu:
- `Units` trong `Sales` luôn bằng 1 trên toàn bộ 675,368 dòng
- Mỗi dòng Sales đại diện cho 1 đơn vị sản phẩm bán ra
- `SUM(Units) = COUNTROWS(Sales)` với dữ liệu hiện tại

Đơn vị tiền tệ chưa được ghi rõ trong nguồn và cần stakeholder xác nhận.

---

## 1.5 Các khái niệm khác

| Thuật ngữ | Định nghĩa | Ghi chú / nguồn |
|-----------|------------|-----------------|
| Actuals | Dữ liệu giao dịch bán hàng thực tế, grain = 1 đơn vị sản phẩm | 675,368 dòng, 2015-01-01 -> 2020-06-30 |
| Budget | Mục tiêu doanh thu theo tháng ở mức Category-Segment | Type = `Budget`, có cho 2020 và 2021 |
| Forecast | Doanh thu dự báo theo tháng ở mức Category-Segment | Type = `Forecast`, chỉ có năm 2021 |
| Budget Attainment | `Actual Revenue / Budget` cùng kỳ, cùng tổ hợp Category-Segment | Chỉ tính được Jan-Jun 2020 (giai đoạn overlap) |
| Forecast Variance | `(Actual - Forecast) / Forecast` | Chưa tính được do thiếu overlap giữa Actual và Forecast |
| Customers Served | Số khách hàng duy nhất phát sinh giao dịch trong kỳ | `DISTINCTCOUNT(CustomerID)` |

---

## 1.6 Time terms

| Term | Definition |
|------|------------|
| YTD | Year-to-date, lũy kế từ đầu năm đến kỳ hiện tại |
| YoY | Year-over-year, so sánh cùng kỳ năm trước |
| MoM | Month-over-month, so sánh với tháng trước |
| Period | Kỳ báo cáo theo tháng/quý trong Budget/Forecast |
