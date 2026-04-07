## 2 — CHUẨN HÓA DỮ LIỆU (DATA NORMALIZATION)
**Bộ dữ liệu:** NYC TLC Yellow Taxi Trip Records (2024)

### 1. Tổng quan
Sau khi hoàn thành bước làm sạch dữ liệu (Data Cleaning), dự án tiếp tục thực hiện chuẩn hóa dữ liệu (Data Normalization) trên bộ dữ liệu NYC TLC Yellow Taxi Trip Records của năm 2024.

**Mục tiêu của bước này gồm:**
* chuẩn hóa các đặc trưng số về cùng thang đo,
* chuẩn hóa dữ liệu dạng text để đảm bảo tính nhất quán,
* chuyển đổi dữ liệu categorical sang dạng số (encoding),
* chuẩn hóa định dạng dữ liệu (datetime, tiền tệ),
* đảm bảo dữ liệu sẵn sàng cho phân tích và mô hình học máy,
* giữ nguyên dữ liệu gốc,
* lưu lại dữ liệu theo từng version sau mỗi bước xử lý,
* code thực hiện có comment rõ ràng theo từng bước.

### 2. Nội dung thực hiện

#### **Chuẩn hoá dữ liệu số (Numeric Scaling) — Version V5.1**
Các thuộc tính số được đưa về cùng khoảng giá trị nhằm tránh sự chênh lệch lớn giữa các đặc trưng.
* **Áp dụng Min-Max Scaling (0–1) cho:** `trip_distance`, `total_amount`
* **Tạo thêm các cột:** `trip_distance_scaled`, `total_amount_scaled`
* **Dữ liệu sau khi xử lý được lưu:** `data/v5/v5_1_scaled.parquet`
* **Code thực hiện (có comment rõ ràng cho từng bước):**
    * load dữ liệu từ V5.0
    * tính toán Min-Max
    * tạo cột mới
    * lưu version mới

#### **Chuẩn hoá dữ liệu dạng text — Version V5.2**
Dữ liệu text được xử lý để đảm bảo tính đồng nhất:
* chuyển về chữ thường (lowercase),
* loại bỏ khoảng trắng dư thừa,
* chuẩn hóa format text.
* **Áp dụng cho:** `store_and_fwd_flag`
* **Dữ liệu sau khi xử lý được lưu:** `data/v5/v5_2_text_cleaned.parquet`
* **Code thực hiện (có comment):**
    * load dữ liệu từ V5.1
    * xử lý lowercase + trim
    * lưu version mới

#### **Encode dữ liệu categorical — Version V5.3**
Dữ liệu dạng phân loại được chuyển sang dạng số để phục vụ tính toán.
* **Label Encoding:** `payment_type`, `RatecodeID`
* **One-hot Encoding:** `pay_credit_card`, `pay_cash`, `pay_no_charge`, `pay_dispute`, `pay_unknown`
* **Dữ liệu sau khi xử lý được lưu:** `data/v5/v5_3_encoded.parquet`
* **Code thực hiện (có comment):**
    * load dữ liệu từ V5.2
    * thực hiện encoding
    * tạo cột mới
    * lưu version

#### **Chuẩn hoá format dữ liệu — Version V5.4**
Chuẩn hóa các định dạng dữ liệu quan trọng.
* **Ngày tháng (datetime):** chuyển về định dạng **TIMESTAMP**, trích xuất: `year`, `month`, `hour`
* **Tiền tệ (currency):** chuyển về kiểu **DOUBLE**, làm tròn 2 chữ số thập phân
* **Áp dụng cho:** `fare_amount`, `tip_amount`, `total_amount`
* **Dữ liệu sau khi xử lý được lưu:** `data/v5/v5_4_formatted.parquet`
* **Code thực hiện (có comment):**
    * convert datetime
    * extract features
    * chuẩn hóa numeric format
    * lưu version

### 3. Kết quả đạt được
Sau bước chuẩn hóa:
* dữ liệu số được đưa về cùng thang đo,
* dữ liệu text được đồng nhất,
* dữ liệu categorical đã được mã hóa,
* định dạng dữ liệu được chuẩn hóa hoàn toàn,
* dữ liệu được lưu lại theo từng version (V5.0 → V5.4),
* toàn bộ code xử lý có comment rõ ràng,
* dataset sẵn sàng cho bước Validation và Output.

---

## 3 — XUẤT DỮ LIỆU (OUTPUT)
**Bộ dữ liệu:** NYC TLC Yellow Taxi Trip Records (2024)

### 1. Tổng quan
Sau khi hoàn thành các bước Data Cleaning và Data Normalization, dự án tiến hành kiểm tra và xuất dữ liệu cuối cùng (Output).

**Mục tiêu của bước này gồm:**
* kiểm tra lại toàn bộ dữ liệu sau xử lý,
* đảm bảo dữ liệu không còn giá trị NULL,
* đảm bảo dữ liệu hợp lệ và nhất quán,
* chuẩn bị dữ liệu sẵn sàng cho phân tích và mô hình học máy,
* xuất dữ liệu ra các định dạng phù hợp (Parquet, CSV),
* lưu lại version cuối cùng của dataset,
* hỗ trợ tải dữ liệu về máy (download),
* code thực hiện có comment rõ ràng theo từng bước.

### 2. Nội dung thực hiện

#### **Kiểm tra dữ liệu (Validation) — Version V6**
Dữ liệu sau chuẩn hóa được kiểm tra lại để đảm bảo chất lượng.
* **Kiểm tra NULL:** `trip_distance`, `total_amount`, `passenger_count`
* **Loại bỏ dữ liệu không hợp lệ:** giá trị âm (distance, amount), dữ liệu thiếu, dữ liệu không consistent.
* **Đảm bảo tính nhất quán:** dữ liệu đúng kiểu (numeric, datetime), không còn lỗi format, categorical đã được encode đầy đủ.
* **Lưu dữ liệu sau validation:** `data/v6/v6_validated.parquet`
* **Code thực hiện (có comment rõ ràng):**
    * load dữ liệu từ V5.4
    * kiểm tra NULL
    * filter dữ liệu không hợp lệ
    * validate kiểu dữ liệu
    * lưu version V6

#### **Xuất dữ liệu (Export) — Version V7**
Dữ liệu sau khi validation được xuất ra các định dạng sử dụng trong thực tế.
* **Định dạng xuất:**
    * **Parquet:** tối ưu cho Big Data, hỗ trợ nén, xử lý nhanh
    * **CSV:** dễ đọc, dùng cho kiểm tra và demo
* **File xuất ra:** `data/v7/final_dataset.parquet`, `data/v7/final_dataset.csv`
* **Code thực hiện (có comment):**
    * load dữ liệu từ V6
    * export sang parquet
    * export sang csv
    * lưu version cuối

#### **Tải dữ liệu về máy (Download)**
Sau khi export, dataset có thể được tải về để sử dụng:
* tải file .parquet để dùng trong pipeline Big Data
* tải file .csv để kiểm tra, demo hoặc trực quan hóa
* **Có thể thực hiện download trực tiếp trong môi trường:** Jupyter Notebook, Google Colab, hoặc copy từ thư mục `data/v7/`

### 3. Kết quả đạt được
Sau bước OUTPUT:
* dữ liệu không còn NULL,
* dữ liệu đã được kiểm tra và đảm bảo tính nhất quán,
* **dataset được lưu theo version:** V6: validated, V7: final output
* **dữ liệu được xuất thành công ra:** Parquet, CSV
* **hỗ trợ tải dữ liệu về máy để sử dụng cho:** phân tích dữ liệu, trực quan hóa, xây dựng mô hình học máy.