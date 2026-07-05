# Data gap & câu hỏi cho stakeholder — VanArsdel Executive Dashboard

Liệt kê các điểm đã xác thực trên dữ liệu và câu hỏi cần chốt với stakeholder trước khi build model/report.


## Bảng data gap tổng hợp

| # | Ảnh hưởng | Vấn đề (đã xác thực trên dữ liệu) | Câu hỏi cho stakeholder | Status | Owner | Due date | Decision |
|---|-----------|-----------------------------------|-------------------------|--------|-------|----------|----------|
| 1 | Cao | Actuals chỉ có đến `2020-06-30` (dừng giữa năm). Budget có cả 2020 và 2021; Forecast chỉ có 2021. Do đó Actual vs Budget chỉ so sánh được Jan-Jun 2020; Actual vs Forecast không so sánh được với dữ liệu hiện có. | VanArsdel có Actuals của nửa cuối 2020 và/hoặc 2021 không? Nếu không, dashboard cần hiển thị rõ "không đủ dữ liệu" thay vì để trống hoặc 0 gây hiểu nhầm. | Closed | BA + Stakeholder | 2026-07-05 | Phiên bản nộp hiện tại chỉ tính Budget Attainment cho Jan-Jun 2020. Ngoài kỳ overlap hiển thị `Insufficient overlap`; không hiển thị Forecast variance. |
| 2 | Cao | Tổ hợp `Rural-Productivity` (2 sản phẩm) có trong Product nhưng không có cột ngân sách tương ứng trong Budget/Forecast (chỉ có `Rural-Select`). | Đây là ngân sách thực sự bị thiếu, hay 2 sản phẩm này được gộp vào tổ hợp khác khi lập ngân sách? Cách xử lý: loại khỏi Budget Attainment hay gán Budget = 0? | Closed | BA + DA | 2026-07-05 | Tạm thời loại `Rural-Productivity` khỏi visual/KPI so sánh ngân sách và gắn footnote dữ liệu thiếu. Không gán Budget = 0 để tránh sai lệch. |
| 3 | Cao | 100% Manufacturer trong Product = `VanArsdel`. Không có dữ liệu đối thủ dù mô hình có cột ManufacturerID gợi ý đa nhà sản xuất. | Xác nhận: dữ liệu này chỉ gồm hàng tự doanh của VanArsdel? Nếu vậy nên bỏ yêu cầu "phân tích cạnh tranh theo Manufacturer" khỏi phạm vi để tránh chart luôn chỉ có 1 giá trị. | Closed | BA + Stakeholder | 2026-07-05 | Scope dashboard chỉ theo dõi kinh doanh nội bộ VanArsdel. Không triển khai visual so sánh đối thủ hoặc market share theo Manufacturer trong phiên bản này. |
| 4 | Trung bình | 112,202 dòng Sales (16.6%, toàn bộ năm 2015) có Date nằm ngoài phạm vi bảng Date (2016-2021). Nếu join bằng inner join, năm 2015 sẽ biến mất khỏi visual theo thời gian mà không có cảnh báo. | Có nên mở rộng Date dimension để phủ 2015, hay chủ động loại 2015 khỏi phạm vi báo cáo (và nêu rõ lý do trong report spec)? | Closed | BA + DA | 2026-07-05 | Chốt phạm vi báo cáo từ `2016-01-01` đến `2020-06-30` cho bản nộp. Toàn bộ visual theo thời gian áp filter date >= `2016-01-01` và có note phạm vi dữ liệu. |
| 5 | Trung bình | Không có cột Revenue/Cost/Discount/Tax sẵn; phải tự tính Units x Unit Price và Units x Unit Cost. Chưa rõ đơn vị tiền tệ (không có ký hiệu). | Xác nhận công thức Revenue = Units x Unit Price có đúng thực tế kinh doanh (không có chiết khấu/khuyến mãi ẩn) không? Đơn vị tiền tệ là gì? | Open | Stakeholder | TBC | TBC |
| 6 | Trung bình | Cột `Email Name` trong Customer ghép chung email và họ tên trong một chuỗi dạng `(email): Họ, Tên`. | DA có cần tách thành 2 cột riêng (Email, Customer Name) để hiển thị tên khách trên dashboard hoặc bảng chi tiết không? | Open | BA + DA | TBC | TBC |
| 7 | Thấp | Có 133 dòng Sales trùng hoàn toàn tổ hợp (ProductID, Date, CustomerID, CampaignID); không có PK kỹ thuật để phân biệt là 2 giao dịch khác nhau hay lỗi nạp trùng. | Đây là 2 giao dịch mua riêng biệt trong cùng ngày (giữ nguyên) hay lỗi trùng lặp ETL (cần loại bỏ)? | Open | DA | TBC | TBC |
| 8 | Thấp | TrafficChannel có khoảng trắng thừa (`Organic Search `) và lỗi chính tả (`Affliliate`); Device có lỗi chính tả (`Deskop`) và giá trị đặc biệt `Paper` (gắn với kênh Mail). | Xác nhận chuẩn hóa ETL: TRIM + sửa `Affliliate -> Affiliate`, `Deskop -> Desktop`. Giá trị `Paper` giữ nguyên như một Device hợp lệ hay gộp nhóm khác? | Open | DA | TBC | TBC |
| 9 | Thấp | Không có thông tin phân khúc khách hàng (Customer Segment) tách biệt với Segment của sản phẩm; hai khái niệm "Segment" dễ gây nhầm khi trao đổi. | Khi nói "Segment" trên dashboard, cần mặc định là Product Segment và ghi rõ nhãn để tránh hiểu nhầm với phân khúc khách hàng (hiện chưa có dữ liệu). | Open | BA | TBC | TBC |
| 10 | Trung bình | Budget/Forecast không có chiều Region/Channel/Customer, chỉ có Category-Segment x Tháng. | VanArsdel có kế hoạch cấp ngân sách chi tiết hơn (theo Region/Channel) trong tương lai không? Nếu không, Budget Attainment chỉ khả dụng ở trang tổng quan Category-Segment. | Open | Stakeholder | TBC | TBC |
| 11 | Trung bình | Không có khái niệm Order ID/Transaction ID; mỗi dòng Sales = 1 sản phẩm bán cho 1 khách tại 1 thời điểm. | Đây có phải giới hạn chấp nhận được (chỉ cần trend và breakdown, không cần basket analysis), hay VanArsdel có dữ liệu đơn hàng chi tiết hơn ở hệ thống khác? | Open | Stakeholder | TBC | TBC |
| 12 | Trung bình | Không có dữ liệu hồ sơ khách hàng ngoài Zip/Email Name; thiếu tuổi, giới tính, ngày đăng ký, hạng thành viên. | "Phân khúc khách hàng" (nếu cần) có chấp nhận định nghĩa dựa trên hành vi mua (frequency, monetary, recency) do BA/DA tự xây thay vì nhân khẩu học không? | Open | Stakeholder | TBC | TBC |

 ## Ưu tiên xử lý trước khi build

1. Bắt buộc chốt ngay: #1, #2, #3
2. Cần chốt trước khi ký report spec: #4, #5, #10, #11
3. Có thể xử lý trong ETL hoặc sprint sau: #6, #7, #8, #9, #12
