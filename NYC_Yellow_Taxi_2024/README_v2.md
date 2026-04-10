# README – Đề tài Big Data: Data Cleaning, Normalization và Dimensionality Reduction bằng SVD

## 1. Thông tin chung

- **Chủ đề:** Dimensionality Reduction: SVD
- **Dataset sử dụng:** NYC Yellow Taxi Trip Records 2024
- **Nguồn dữ liệu gốc:** các file parquet theo tháng của bộ dữ liệu Yellow Taxi 2024
- **Môi trường chạy khuyến nghị:** Google Colab

Notebook này được thiết kế theo đúng 2 nhiệm vụ chính:

- **phần 1:** Data Cleaning & Normalization  
- **phần 2 :** Feature Matrix Preparation & Dimensionality Reduction using Truncated SVD

---

## 2. Mục tiêu của bài làm

Bài làm không chỉ dừng ở tiền xử lý dữ liệu, mà được xây thành một pipeline hoàn chỉnh để phục vụ trực tiếp cho đề tài **Dimensionality Reduction: SVD**.

### phần 1 – Data Cleaning & Normalization
Mục tiêu:
- Chọn bộ dữ liệu lớn, phù hợp với bài toán Big Data
- Làm sạch dữ liệu: xử lý null, duplicate, dữ liệu sai
- Chuẩn hoá kiểu dữ liệu và format
- Tạo ra một phiên bản dữ liệu sạch, nhất quán, sẵn sàng cho bước xây ma trận đặc trưng

### phần 2 – SVD
Mục tiêu:
- Chọn các đặc trưng phù hợp cho mô hình giảm chiều
- Biến đổi dữ liệu thành **feature matrix**
- Áp dụng **Truncated SVD** để giảm số chiều
- So sánh kích thước trước và sau giảm chiều
- Lưu dữ liệu đầu ra để dùng cho các bước phân tích tiếp theo

---

## 3. Vì sao chọn NYC Yellow Taxi 2024

Bộ dữ liệu này phù hợp vì:
- Kích thước lớn, gồm 12 file dữ liệu theo tháng
- Nhiều bản ghi, đúng tinh thần Big Data
- Có cấu trúc trường dữ liệu rõ ràng
- Có đủ dữ liệu số, dữ liệu phân loại, dữ liệu thời gian
- Có thể xây được ma trận đặc trưng lớn để áp dụng SVD

Dù bộ dữ liệu này không phải text dataset, nó vẫn phù hợp để minh hoạ đầy đủ pipeline:
**raw data → cleaning → normalization → encoding/scaling → feature matrix → Truncated SVD**

---

## 4. Cấu trúc thư mục và version dữ liệu

Notebook tuân thủ nguyên tắc:
- **Không sửa dữ liệu gốc**
- **Mỗi bước lưu một version riêng**
- Có thể kiểm tra lại và tái lập toàn bộ quy trình

### Cấu trúc version
- `raw/` : dữ liệu gốc tải về
- `v1_loaded/` : dữ liệu gốc sau khi gom và lưu Version 1
- `v2_cleaned_nulls/` : sau xử lý giá trị thiếu
- `v3_deduplicated/` : sau xóa bản ghi trùng
- `v4_validated/` : sau loại dữ liệu bất hợp lệ
- `v5_normalized/` : sau chuẩn hoá dữ liệu và feature engineering
- `v6_svd_input/` : dữ liệu đầu vào cho SVD
- `v7_svd_output/` : dữ liệu sau khi giảm chiều
- `final/` : file kết quả cuối của Task 1
- `reports/` : các file báo cáo kiểm tra null, duplicate, invalid, SVD

---

## 5. Nội dung xử lý chính

## phần 1 – Data Cleaning & Normalization

### Bước 1: Tải dữ liệu gốc
- Tải 12 file parquet của năm 2024
- Tạo cấu trúc thư mục dự án
- Bảo toàn dữ liệu gốc

### Bước 2: Đọc schema và lưu Version 1
- Đọc schema toàn bộ dữ liệu
- Xem mẫu dữ liệu
- Đếm số dòng
- Lưu toàn bộ dữ liệu vào `v1_loaded.parquet`

### Bước 3: Xử lý giá trị thiếu – Version 2
- Kiểm tra null ở các cột quan trọng
- Loại bỏ các dòng thiếu dữ liệu lõi
- Điền giá trị hợp lý cho một số cột:
  - `store_and_fwd_flag` → `'N'`
  - `passenger_count` → `1`
  - `RatecodeID` → `1`

### Bước 4: Xóa duplicate – Version 3
- Báo cáo số dòng trùng
- Dùng `SELECT DISTINCT *`
- Lưu lại phiên bản đã loại duplicate

### Bước 5: Loại dữ liệu sai – Version 4
- Loại các dòng có:
  - dropoff trước pickup
  - giá trị âm bất hợp lý
  - total_amount âm quá lớn
- Giữ lại dữ liệu hợp lệ

### Bước 6: Chuẩn hoá dữ liệu – Version 5
- Đổi tên cột về dạng rõ nghĩa, nhất quán
- Ép kiểu dữ liệu về numeric / timestamp
- Chuẩn hóa `store_and_fwd_flag`
- Làm tròn các cột tiền tệ 2 chữ số
- Tạo thêm đặc trưng thời gian:
  - năm, tháng, ngày, giờ, thứ
  - thời lượng chuyến đi theo phút

### Bước 7: Kiểm tra cuối và xuất dữ liệu Task 1
- Kiểm tra lại null
- Kiểm tra lại duplicate
- Xuất dữ liệu sạch ra:
  - Parquet
  - CSV

---

## phần 2 – Feature Matrix Preparation & Truncated SVD

### Bước 8: Chọn dữ liệu đầu vào cho SVD – Version 6
- Lấy mẫu dữ liệu lớn nhưng phù hợp RAM
- Chọn các cột số và cột phân loại phục vụ tạo feature matrix

### Bước 9: Tạo feature matrix
- Chuẩn hoá cột số bằng `StandardScaler`
- Mã hoá cột categorical bằng `OneHotEncoder`
- Kết hợp bằng `ColumnTransformer`
- Thu được ma trận đặc trưng dạng sparse

### Bước 10: Giảm chiều bằng Truncated SVD
- Chọn số thành phần `n_components = 20`
- Áp dụng `TruncatedSVD`
- So sánh kích thước ma trận trước và sau giảm chiều
- Tính `explained_variance_ratio`
- Tính tổng phương sai giải thích

### Bước 11: Xuất kết quả cuối
- Xuất dữ liệu sau giảm chiều ra Parquet và CSV
- Xuất báo cáo `svd_report.json`

---

## 6. Giải thích vì sao dùng Truncated SVD thay vì SVD đầy đủ

Trong bài toán Big Data, sau bước mã hoá categorical bằng one-hot, ma trận đặc trưng thường:
- có số chiều rất lớn
- thưa (sparse)
- nặng về RAM và thời gian tính toán

Vì vậy, notebook sử dụng **Truncated SVD** thay vì full SVD vì:
- phù hợp hơn với ma trận sparse
- thực tế hơn trong môi trường Big Data
- giảm chi phí tính toán
- vẫn giữ được phần lớn thông tin quan trọng nếu chọn số thành phần phù hợp

---

## 7. Đầu ra của bài làm

### Kết quả Task 1
- `task1_cleaned_normalized_yellow_taxi_2024.parquet`
- `task1_cleaned_normalized_yellow_taxi_2024.csv`

### Kết quả Task 2
- `v7_reduced_svd.parquet`
- `v7_reduced_svd.csv`
- `svd_report.json`

### Các file báo cáo
- `null_report_before.csv`
- `duplicate_report_before.csv`
- `duplicate_report_after.csv`
- `invalid_report_before.csv`
- `final_null_check.csv`
- `final_duplicate_check.csv`

---

## 8. Cách chạy notebook

### Bước 1
Mở notebook trên Google Colab.

### Bước 2
Chạy lần lượt từ trên xuống dưới từng cell.

### Bước 3
Đợi notebook:
- tải dữ liệu
- xử lý Task 1
- chạy Task 2
- xuất file kết quả

### Bước 4
Dùng cell cuối để tải file kết quả về máy.

---

## 9. Lưu ý khi chạy

- Dataset lớn nên thời gian tải dữ liệu có thể lâu.
- Task 2 có tham số:
  - `SVD_SAMPLE_ROWS = 2_000_000`
- Nếu Colab thiếu RAM, có thể giảm tham số này xuống.
- Nếu Colab đủ mạnh, có thể tăng lên để kết quả đại diện hơn.

---

