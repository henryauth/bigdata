# Dự án Phân tích dữ liệu FIFA 23 – Giảm chiều dữ liệu bằng SVD
## 1. GIỚI THIỆU DỰ ÁN

Trong lĩnh vực Big Data, dữ liệu thực tế thường có các đặc điểm:
Kích thước rất lớn (GB đến hàng chục GB)
Nhiều thuộc tính (high-dimensional data)
Dữ liệu nhiễu, thiếu và không đồng nhất
Có nhiều biến tương quan cao

👉 Với dataset FIFA 23 (players + teams), bài toán gặp vấn đề:
Hơn 100+ features cho mỗi bảng
Dữ liệu bị phân tán và dư thừa thông tin
Khó trực quan hóa và khó đưa vào mô hình Machine Learning

🎯 Mục tiêu dự án
Dự án này xây dựng một pipeline xử lý dữ liệu hoàn chỉnh nhằm:
Làm sạch dữ liệu (Data Cleaning)
Chuẩn hoá dữ liệu (Data Standardization)
Kiểm tra và loại bỏ dữ liệu lỗi (Data Validation)
Loại bỏ dữ liệu trùng lặp (Deduplication)
Giảm chiều dữ liệu bằng SVD (Dimensionality Reduction)

👉 Kết quả cuối cùng:
Dữ liệu gọn hơn – sạch hơn – phù hợp cho Machine Learning

---

## 2. DATASET SỬ DỤNG

Dự án sử dụng 2 bộ dữ liệu chính:
male_players.csv: Thông tin chi tiết cầu thủ FIFA 23
male_teams.csv: Thông tin đội bóng

**Đặc điểm dữ liệu:**
Kích thước: hơn 3GB
Số lượng bản ghi: hàng chục nghìn đến hàng triệu dòng
Số lượng thuộc tính: ~100+ cột mỗi bảng
Dữ liệu gồm:
Numeric features (overall, pace, shooting,...)
Categorical features (club_name, nationality,...)
Date features (dob, fifa_update_date,...)

👉 Đây là dạng dữ liệu rất phù hợp để áp dụng Big Data processing và Dimensionality Reduction.

---

## 3. XỬ LÝ DỮ LIỆU
### 3.1 Load dữ liệu (Bronze Layer)
Đọc dữ liệu từ file CSV vào Spark DataFrame
Giữ nguyên dữ liệu gốc để đảm bảo tính toàn vẹn

👉 Output: dữ liệu thô (raw data)

### 3.2 Data Cleaning (Silver Layer)
Vấn đề xử lý:
Missing values (NULL)
Dữ liệu không đầy đủ ở nhiều cột quan trọng
Cách xử lý:
Numeric columns: thay bằng giá trị trung bình (mean)
Categorical columns: thay bằng "Unknown"
Date columns: chuẩn hoá format datetime

👉 Kết quả:
Không còn giá trị NULL quan trọng
Dữ liệu ổn định hơn để xử lý tiếp

---

### 3.3 Deduplication (Xoá dữ liệu trùng)
Xoá bản ghi trùng theo:
player_id (players)
team_id (teams)

👉 Kết quả:
Loại bỏ dữ liệu dư thừa
Đảm bảo mỗi thực thể chỉ xuất hiện 1 lần

---

### 3.4 Data Validation (Loại bỏ dữ liệu sai)
**Các lỗi được xử lý:**
Giá trị âm bất hợp lý:
value_eur < 0
wage_eur < 0
age < 0
height_cm ≤ 0
weight_kg ≤ 0
Dữ liệu lỗi định dạng:
ngày tháng sai format
numeric bị parse lỗi

👉 Kết quả:
Players: ~5.6M → sau lọc còn dữ liệu hợp lệ
Teams: dữ liệu sạch và thống nhất

---

### 3.5 Data Standardization (Chuẩn hoá kiểu dữ liệu)
String → Numeric (overall, pace, shooting,...)
String → Date (dob, fifa_update_date,...)
Giữ lại đúng schema Spark chuẩn:
✔ Integer
✔ Double
✔ Date
✔ String

👉 Kết quả:
Dataset đồng nhất kiểu dữ liệu
Sẵn sàng cho feature engineering

---

## 4. GOLD LAYER – DATA READY FOR ML
Sau khi xử lý:
Dataset đạt trạng thái:
✔ Không null
✔ Không duplicate
✔ Không dữ liệu sai
✔ Kiểu dữ liệu chuẩn

👉 Đây là dataset “Gold Layer” trong kiến trúc dữ liệu

---

## 5. DIMENSIONALITY REDUCTION – SVD
**Vấn đề ban đầu**

Dataset FIFA 23 có:
Hơn 100+ features
Nhiều thuộc tính có tương quan mạnh
Dữ liệu bị nhiễu (noise)
Khó training Machine Learning model

**Hậu quả:**
Tốn tài nguyên tính toán
Dễ overfitting
Model chạy chậm

**Giải pháp: SVD (Singular Value Decomposition)**
SVD là kỹ thuật giảm chiều dữ liệu bằng cách phân rã ma trận:

X = U Σ Vᵀ

Trong đó:

U: biểu diễn dữ liệu gốc trong không gian mới
Σ: mức độ quan trọng của từng thành phần
Vᵀ: vector đặc trưng

**Mục tiêu áp dụng SVD**
Giảm số chiều dữ liệu từ ~100+ → k chiều (ví dụ 10–20)
Giữ lại thông tin quan trọng nhất
Loại bỏ noise và redundancy
Tăng tốc độ training ML model

**Kết quả sau SVD**
Players dataset:
Input: ~100+ features
Output: vector dạng:
svd_features = [f1, f2, f3, ...]

Teams dataset:
Input: ~30–50 features
Output:
svd_features = [f1, f2]

**Lợi ích đạt được**
✔ Giảm chiều dữ liệu đáng kể
✔ Giữ lại pattern quan trọng trong dữ liệu
✔ Giảm overfitting
✔ Tăng tốc độ xử lý
✔ Tối ưu cho ML pipeline

## 6. KẾT LUẬN
Dự án đã xây dựng thành công pipeline Big Data gồm:

Bronze: dữ liệu gốc
Silver: làm sạch + chuẩn hoá
Gold: dữ liệu sẵn sàng ML
Model: SVD giảm chiều dữ liệu
