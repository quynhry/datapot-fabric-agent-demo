# Business Requirements Document (BRD)
## VanArsdel Executive Dashboard

**Version:** 0.1 — Draft  
**Date:** *(04/07/2026)*  
**Author:** *(Nguyễn Như Quỳnh)*  
**Reviewer:** *(tên stakeholder VanArsdel)*

---

## 1. Bối cảnh & mục tiêu

VanArsdel là công ty bán lẻ cần một **dashboard điều hành** để theo dõi tình hình kinh doanh theo thời gian thực. Hiện tại, ban lãnh đạo không có một nơi thống nhất để xem kết quả so với ngân sách và nhận diện vùng/sản phẩm cần can thiệp.

**Mục tiêu của dashboard:**
- Cung cấp bức tranh tổng quan về doanh thu thực tế so với ngân sách ở giai đoạn có thể đối chiếu.
- Phát hiện nhanh khu vực / sản phẩm / kênh lưu lượng đang dưới hoặc vượt kế hoạch.
- Theo dõi xu hướng tăng trưởng qua các năm trong phạm vi dữ liệu hợp lệ theo Date dimension.



## 2. Người dùng & quyết định hỗ trợ

| Vai trò | Câu hỏi chính họ cần trả lời | Hành động ra quyết định |
|---------|------------------------------|------------------------|
| **CEO / C-level** | Doanh thu tổng công ty đang đi đúng hướng không? Budget attainment YTD? | Điều chỉnh chiến lược, phê duyệt ngân sách |
| **VP Sales / Sales Director** | Segment / Category nào đang under-perform? Khu vực nào cần tăng cường? | Phân bổ lại nhân lực / khuyến mãi |
| **Marketing Manager** | Tổ hợp Traffic Channel x Device nào tạo doanh thu hiệu quả nhất? | Tối ưu phân bổ kênh và nội dung |
| **Regional Manager** | Vùng của tôi đang đứng đâu so với target và so với các vùng khác? | Hành động cụ thể tại địa phương |



## 3. Câu hỏi nghiệp vụ mà dashboard phải trả lời

1. Doanh thu thực tế tháng này / YTD so với Budget như thế nào trong giai đoạn có overlap dữ liệu?
2. Sản phẩm / Segment / Category nào đóng góp nhiều nhất vào doanh thu?
3. Khu vực địa lý nào có hiệu suất tốt nhất / kém nhất?
4. Xu hướng doanh thu qua các năm 2015–2020 như thế nào? Tháng nào theo mùa vụ?
5. Những tổ hợp Traffic Channel x Device nào tương quan với spike doanh thu?
6. Dữ liệu hiện có có đủ để so sánh Forecast variance hay không?
7. Budget attainment hiện tại ở mức nào, và có khả năng đạt mục tiêu năm không?



## 4. KPI & định nghĩa metric

| KPI | Định nghĩa nghiệp vụ | Công thức (business logic) | Đơn vị | Target / Benchmark |
|-----|---------------------|---------------------------|--------|-------------------|
| **Total Revenue** | Tổng doanh thu thực tế từ bán hàng | `SUM(Units × Unit Price)` | Currency | Theo Budget |
| **Budget Attainment %** | Tỷ lệ doanh thu thực tế so với ngân sách | `Actual Revenue ÷ Budget Amount` | % | ≥ 100% |
| **Revenue Variance vs Budget** | Chênh lệch tuyệt đối thực tế − ngân sách | `Actual − Budget` | Currency | ≥ 0 |
| **Revenue YoY %** | Tăng trưởng doanh thu so cùng kỳ năm ngoái | `(Revenue_thisYear − Revenue_lastYear) ÷ Revenue_lastYear` | % | > 0% |
| **Revenue YTD** | Doanh thu lũy kế từ đầu năm đến kỳ hiện tại | `SUM(Revenue) từ Jan 1 đến ngày hiện tại` | Currency | Theo Budget YTD |
| **Total Units Sold** | Tổng số lượng sản phẩm bán ra | `SUM(Units)` | Units | Theo kế hoạch volume |
| **Total Cost** | Tổng giá vốn hàng bán | `SUM(Units × Unit Cost)` | Currency | — |
| **Gross Profit** | Lợi nhuận gộp | `Revenue − Cost` | Currency | — |
| **Gross Margin %** | Biên lợi nhuận gộp | `Gross Profit ÷ Revenue` | % | — |
| **Top N Products** | N sản phẩm có doanh thu cao nhất trong kỳ | Rank by Revenue DESC | — | — |

**Giả định cần xác nhận:** Revenue = Units × Unit Price phản ánh đúng thực tế kinh doanh (không có discount/tax ẩn), và đơn vị tiền tệ chuẩn của báo cáo.



## 5. Phạm vi (Scope)

### In scope
- Dữ liệu thực tế (Actuals): `2015-01-01` -> `2020-06-30`.
- Ngân sách & Forecast: các kỳ có trong `VanArsdel_Budget_Forecast.xlsx`.
- Đối chiếu Budget Attainment ở giai đoạn có overlap dữ liệu (Jan-Jun 2020).
- Phân tích theo: thời gian, sản phẩm (Category/Segment), địa lý (Country/State/City), Traffic Channel x Device.
- Dashboard tĩnh, refresh thủ công.
- Phạm vi thời gian hiển thị trên dashboard: `2016-01-01` -> `2020-06-30`.

### Out of scope
- So sánh cạnh tranh theo Manufacturer (dataset hiện chỉ có VanArsdel).
- Forecast variance (không có overlap Actual vs Forecast ở cùng kỳ).
- Phân tích khách hàng cá nhân / customer journey.
- Dự báo tương lai (ML forecast) — chỉ dùng Budget/Forecast có sẵn.
- Row-level security theo từng Regional Manager *(có thể thêm sau)*.



## 6. Giả định

1. Revenue = Units × Unit Price; Cost = Units × Unit Cost.
2. Năm tài chính = Năm dương lịch (Jan–Dec).
3. Đơn vị tiền tệ đồng nhất — chờ stakeholder xác nhận ký hiệu chuẩn.
4. Budget join với Sales qua Year × Month × Segment × Category.
5. Dashboard audience chính: C-level và VP Sales.
6. Chốt loại năm 2015 khỏi phiên bản nộp hiện tại để đảm bảo nhất quán với Date dimension.



## 7. Non-functional requirements

- **Tốc độ load:** < 5 giây cho mỗi page (Import mode).
- **Kích thước page:** 1280 × 720 px (16:9).
- **Theme:** `shared/themes/datapot-theme.json`.
- **Ngôn ngữ label:** Tiếng Anh (hoặc theo yêu cầu VanArsdel).



## 8. KPI applicability rules (bắt buộc khi build)

1. **Budget Attainment %** chỉ hiển thị khi kỳ thời gian có overlap Actual vs Budget.
2. Nếu ngoài kỳ overlap (ví dụ Jul-2020 trở đi), KPI/visual phải hiển thị nhãn `Insufficient overlap` thay vì `0` hoặc blank không giải thích.
3. **Forecast variance** nằm ngoài scope phiên bản hiện tại và không hiển thị trong dashboard chính.
4. Visual so sánh ngân sách theo Category-Segment phải có chú thích xử lý tổ hợp thiếu ngân sách (`Rural-Productivity`).
5. Tất cả visual theo thời gian áp dụng phạm vi từ `2016-01-01` và có ghi chú phạm vi dữ liệu.
