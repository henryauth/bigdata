# TASK 1 — LÀM SẠCH DỮ LIỆU & CHUẨN HÓA DỮ LIỆU  
## Bộ dữ liệu NYC TLC Yellow Taxi Trip Records (2024)

---

## 1. Tổng quan dự án

Dự án này thực hiện **làm sạch dữ liệu (Data Cleaning)** và **chuẩn hóa dữ liệu (Data Normalization)** trên bộ dữ liệu **NYC TLC Yellow Taxi Trip Records** của năm **2024**.

Mục tiêu của bài toán gồm:

- tìm một bộ dữ liệu phù hợp có kích thước lớn (**lớn hơn 3GB**),
- đọc hiểu các trường dữ liệu trong bộ dữ liệu,
- làm sạch dữ liệu,
- chuẩn hóa dữ liệu,
- giữ nguyên dữ liệu gốc,
- lưu lại dữ liệu theo từng phiên bản sau mỗi bước xử lý,
- xuất dữ liệu cuối cùng ở định dạng **Parquet** và **CSV**.

Dự án được xây dựng để chuẩn bị dữ liệu sẵn sàng cho các bước tiếp theo như phân tích dữ liệu, trực quan hóa, xây dựng dashboard hoặc mô hình học máy.

---

## 2. Thông tin bộ dữ liệu

### 2.1. Tên bộ dữ liệu
**NYC TLC Yellow Taxi Trip Records**

### 2.2. Nguồn dữ liệu
Dữ liệu được lấy từ nguồn công khai chính thức của **New York City Taxi and Limousine Commission (TLC)**.

### 2.3. Loại dữ liệu
Đây là bộ dữ liệu **giao thông / bản ghi chuyến đi taxi**, chứa thông tin chi tiết về các chuyến taxi vàng tại thành phố New York.

### 2.4. Định dạng dữ liệu
Dữ liệu gốc được cung cấp dưới dạng **Parquet**, tách theo từng tháng.

### 2.5. Khoảng thời gian sử dụng trong dự án
Dự án sử dụng **12 file dữ liệu Yellow Taxi của năm 2024**, bao gồm:

- Tháng 1/2024
- Tháng 2/2024
- Tháng 3/2024
- Tháng 4/2024
- Tháng 5/2024
- Tháng 6/2024
- Tháng 7/2024
- Tháng 8/2024
- Tháng 9/2024
- Tháng 10/2024
- Tháng 11/2024
- Tháng 12/2024

### 2.6. Quy mô dữ liệu
Dự án sử dụng toàn bộ dữ liệu theo 12 tháng để đảm bảo quy mô đủ lớn, đáp ứng yêu cầu bài toán là **trên 3GB** trong quá trình tải, xử lý và xuất dữ liệu.

> Lưu ý: dung lượng chính xác có thể thay đổi theo thời điểm do dữ liệu nguồn và cách nén file, nhưng việc sử dụng trọn năm dữ liệu giúp đảm bảo xử lý trên bộ dữ liệu lớn.

---

## 3. Lý do chọn bộ dữ liệu này

Bộ dữ liệu này được chọn vì:

- là bộ dữ liệu thực tế từ nguồn chính thức,
- có quy mô lớn với hàng triệu bản ghi,
- phù hợp với yêu cầu xử lý dữ liệu lớn,
- có đủ nhiều kiểu dữ liệu: số, phân loại, thời gian,
- có các vấn đề dữ liệu thực tế như thiếu dữ liệu, trùng lặp, sai logic, sai định dạng,
- rất phù hợp để xây dựng một pipeline làm sạch và chuẩn hóa dữ liệu hoàn chỉnh.

---

## 4. Mô tả dữ liệu

Bộ dữ liệu Yellow Taxi Trip Records chứa thông tin chi tiết cho từng chuyến taxi.

Mỗi dòng dữ liệu thường tương ứng với **một chuyến đi taxi**.

Dữ liệu bao gồm các thông tin như:

- thời gian đón khách và trả khách,
- mã vị trí đón và vị trí trả,
- quãng đường di chuyển,
- số hành khách,
- tiền cước,
- phụ phí,
- phương thức thanh toán,
- tổng tiền,
- thông tin nhà cung cấp dữ liệu chuyến đi.

---

## 5. Các trường chính trong bộ dữ liệu

Dưới đây là các cột quan trọng được sử dụng trong dự án.

### 5.1. Nhóm thông tin nhà cung cấp và metadata chuyến đi
- **VendorID**  
  Mã nhà cung cấp hệ thống ghi nhận dữ liệu chuyến đi.

- **RatecodeID**  
  Mã loại cước áp dụng cho chuyến đi.

- **store_and_fwd_flag**  
  Cờ cho biết dữ liệu chuyến đi có được lưu tạm trên xe trước khi gửi đi hay không.

### 5.2. Nhóm thời gian
- **tpep_pickup_datetime**  
  Ngày giờ bắt đầu chuyến đi.

- **tpep_dropoff_datetime**  
  Ngày giờ kết thúc chuyến đi.

### 5.3. Nhóm hành khách và quãng đường
- **passenger_count**  
  Số lượng hành khách trên xe.

- **trip_distance**  
  Quãng đường chuyến đi, đơn vị dặm.

### 5.4. Nhóm vị trí
- **PULocationID**  
  Mã khu vực đón khách.

- **DOLocationID**  
  Mã khu vực trả khách.

### 5.5. Nhóm thanh toán và chi phí
- **payment_type**  
  Phương thức thanh toán.

- **fare_amount**  
  Tiền cước cơ bản của chuyến đi.

- **extra**  
  Phụ phí bổ sung.

- **mta_tax**  
  Thuế MTA.

- **tip_amount**  
  Tiền tip.

- **tolls_amount**  
  Phí cầu đường.

- **improvement_surcharge**  
  Phụ phí cải thiện dịch vụ.

- **congestion_surcharge**  
  Phụ phí ùn tắc.

- **Airport_fee / airport_fee**  
  Phí sân bay.

- **total_amount**  
  Tổng số tiền phải thanh toán của chuyến đi.

---

## 6. Đặc điểm dữ liệu

Bộ dữ liệu này có nhiều loại biến khác nhau.

### 6.1. Biến số
Ví dụ:
- passenger_count
- trip_distance
- fare_amount
- extra
- mta_tax
- tip_amount
- tolls_amount
- improvement_surcharge
- congestion_surcharge
- airport_fee
- total_amount

### 6.2. Biến phân loại
Ví dụ:
- VendorID
- RatecodeID
- store_and_fwd_flag
- payment_type
- PULocationID
- DOLocationID

### 6.3. Biến thời gian
Ví dụ:
- tpep_pickup_datetime
- tpep_dropoff_datetime

---

## 7. Các vấn đề chất lượng dữ liệu thường gặp

Bộ dữ liệu này phù hợp cho bài toán làm sạch vì có thể tồn tại các vấn đề như:

- giá trị thiếu (`NULL`, `NaN`) ở một số cột,
- dòng dữ liệu trùng lặp,
- giá trị âm không hợp lý ở các cột số,
- giá trị text không đồng nhất,
- sai logic thời gian, ví dụ thời gian trả khách nhỏ hơn thời gian đón khách,
- giá trị ngoại lệ quá lớn hoặc không thực tế.

Do đó, bộ dữ liệu cần được làm sạch trước khi dùng cho phân tích.

---

## 8. Mục tiêu của bước làm sạch dữ liệu

Bước làm sạch tập trung vào các nội dung sau:

### 8.1. Xử lý dữ liệu thiếu
- kiểm tra các giá trị thiếu,
- xóa các dòng thiếu dữ liệu ở những cột quan trọng,
- điền giá trị hợp lý cho các cột phụ nếu có thể.

### 8.2. Xóa dữ liệu trùng lặp
- phát hiện các dòng bị trùng,
- loại bỏ các bản ghi trùng để đảm bảo tính duy nhất.

### 8.3. Loại bỏ dữ liệu sai
- loại bỏ các giá trị âm không hợp lý,
- loại bỏ các dòng sai logic thời gian,
- loại bỏ các bản ghi có giá trị bất thường rõ rệt.

### 8.4. Đồng nhất kiểu dữ liệu
- chuyển các cột dạng chuỗi về kiểu số khi cần,
- chuyển các cột ngày giờ về kiểu datetime,
- chuẩn hóa các cột text.

---

## 9. Mục tiêu của bước chuẩn hóa dữ liệu

Bước chuẩn hóa nhằm làm cho dữ liệu có cấu trúc nhất quán hơn.

Bao gồm:

- chuyển kiểu dữ liệu về đúng chuẩn,
- chuẩn hóa các giá trị phân loại dạng text,
- làm tròn các cột tiền tệ,
- tạo thêm các cột thời gian phục vụ phân tích sau này.

Ví dụ:
- `string -> number`
- `date -> datetime`
- `store_and_fwd_flag -> Y/N`
- thêm các cột `pickup_year`, `pickup_month`, `pickup_day`, `pickup_hour`

---

## 10. Nguyên tắc xử lý dữ liệu trong dự án

Dự án tuân thủ 3 nguyên tắc quan trọng:

### 10.1. Không sửa dữ liệu gốc
Dữ liệu trong thư mục `raw/` chỉ được dùng để đọc, **không được chỉnh sửa trực tiếp**.

### 10.2. Mỗi bước phải lưu lại một phiên bản
Sau mỗi bước xử lý, dữ liệu được lưu thành một phiên bản mới để dễ truy vết.

### 10.3. Code phải có comment rõ ràng
Mỗi bước chính trong notebook đều có comment giải thích:
- bước đó làm gì,
- tại sao cần làm,
- output được lưu ở đâu.

---

## 11. Pipeline dữ liệu theo version

Dự án sử dụng cấu trúc version như sau:

```text
project_bigdata_task1/
├── data/
│   ├── raw/
│   ├── v1_loaded/
│   ├── v2_cleaned_nulls/
│   ├── v3_deduplicated/
│   ├── v4_validated/
│   ├── v5_normalized/
│   └── final/
└── reports/

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
