FIFA23 – DATA PREPROCESSING & DIMENSIONALITY REDUCTION (SVD)
##1. GIỚI THIỆU

Trong bài toán Big Data, dữ liệu thường có kích thước rất lớn (>GB), chứa nhiều lỗi như:

Thiếu dữ liệu (missing values)
Dữ liệu trùng lặp (duplicate)
Dữ liệu sai (invalid values)
Không đồng nhất kiểu dữ liệu

Dự án này thực hiện toàn bộ pipeline xử lý dữ liệu gồm:

Data Cleaning
Data Normalization
Data Validation
Dimensionality Reduction (SVD)

👉 Mục tiêu cuối cùng:

Làm sạch dữ liệu
Chuẩn hoá dữ liệu
Giảm chiều dữ liệu
Chuẩn bị cho Machine Learning
##2. DATASET

Dữ liệu sử dụng:

male_players.csv – thông tin cầu thủ
male_teams.csv – thông tin đội bóng

Đặc điểm:

Kích thước: > 3GB
Số lượng bản ghi lớn (hàng chục nghìn dòng)
Số lượng cột rất nhiều (~100+ cột)

👉 Đây là dữ liệu có độ phức tạp cao → phù hợp cho Big Data

##3. QUY TRÌNH XỬ LÝ DỮ LIỆU
###🔹 V1 – LOAD DATA
Mục tiêu:
Đọc dữ liệu từ file CSV vào Spark DataFrame
Kết quả:
Players: ~56,880 dòng
Teams: ~1,112 dòng

👉 Đây là dữ liệu gốc, chưa xử lý

###🔹 V2 – DATA CLEANING
Mục tiêu:

Xử lý dữ liệu thiếu (missing values)

Các vấn đề gặp phải:
Nhiều cột bị NULL
Một số cột quan trọng bị thiếu dữ liệu
Cách xử lý:
1. Với dữ liệu số:
Không fill = 0 (vì sai logic)
Thay bằng giá trị trung bình (mean)
2. Với dữ liệu dạng chuỗi:
Thay bằng "Unknown"
Kết quả:
Không còn giá trị NULL
Dữ liệu hợp lý hơn (không bị sai lệch)
###🔹 V3 – REMOVE DUPLICATES
Mục tiêu:

Xoá dữ liệu trùng lặp

Cách làm:
Kiểm tra trùng theo:
player_id
team_id
Kết quả:
Không còn bản ghi trùng
Dữ liệu sạch hơn

👉 Output:

player_id: không có duplicate  
team_id: không có duplicate


###🔹 V4 – DATA VALIDATION (LOẠI BỎ DỮ LIỆU SAI)
Mục tiêu:

Loại bỏ dữ liệu không hợp lệ

Các lỗi cần xử lý:
1. Giá trị âm bất hợp lý:
age < 0 ❌
height_cm ≤ 0 ❌
weight_kg ≤ 0 ❌
value_eur < 0 ❌
2. Dữ liệu lỗi format:
ngày sai định dạng
số bị parse sai
Kết quả:
Players: 56,880 dòng
Teams: 1,112 dòng

👉 Dữ liệu hợp lệ, không còn giá trị sai

###🔹 V5 – DATA NORMALIZATION (CHUẨN HOÁ KIỂU DỮ LIỆU)
Mục tiêu:

Đưa dữ liệu về đúng kiểu

Các chuyển đổi:
1. String → Number
Các cột số (overall, pace, shooting...) → numeric
2. Date → Datetime
fifa_update_date → date
dob → date
club_joined_date → date
Kết quả:

✔️ Schema đúng chuẩn:

numeric → double / integer
date → date
string → string

👉 Dữ liệu đã sẵn sàng cho ML

##4. DIMENSIONALITY REDUCTION – SVD
🔥 Vấn đề của dữ liệu:

Dataset có:

Rất nhiều cột (~100+ features)
Nhiều thuộc tính tương quan (correlated)
Dữ liệu rất nặng → xử lý chậm

👉 Gây ra:

Tốn tài nguyên
Model học chậm
Dễ overfitting
💡 Giải pháp: SVD (Singular Value Decomposition)

SVD giúp:

Giảm số chiều dữ liệu
Giữ lại thông tin quan trọng nhất
📌 Nguyên lý:

Ma trận dữ liệu X được phân rã thành:

X = U Σ Vᵀ

Trong đó:

U: ma trận người dùng (players)
Σ: giá trị đặc trưng (singular values)
Vᵀ: ma trận đặc trưng
🎯 Mục tiêu sử dụng SVD:
Giảm số lượng feature
Loại bỏ noise
Tăng tốc độ xử lý
🚀 Sau khi áp dụng SVD:
Trước:
~100+ features
Sau:
chỉ còn k features (ví dụ: 10–20)
✅ Lợi ích đạt được:
1. Giảm chiều dữ liệu

→ nhẹ hơn, nhanh hơn

2. Giữ lại thông tin quan trọng

→ không mất nhiều ý nghĩa dữ liệu

3. Tăng hiệu suất mô hình

→ train nhanh hơn

4. Giảm overfitting

→ model tổng quát tốt hơn

##5. KẾT LUẬN

Sau toàn bộ pipeline:

✔️ Dữ liệu đã được:

Làm sạch (clean)
Chuẩn hoá (normalized)
Kiểm tra lỗi (validated)
Giảm chiều (SVD)

✔️ Dataset cuối:

Sạch
Nhẹ
Chuẩn ML-ready
##6. TỔNG KẾT VERSION
Version	Nội dung
V1	Load dữ liệu
V2	Xử lý missing
V3	Xoá duplicate
V4	Loại dữ liệu sai
V5	Chuẩn hoá kiểu dữ liệu
SVD	Giảm chiều dữ liệu