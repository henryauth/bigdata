# README  — FIFA23 BIGDATA 
## Data Cleaning • Data Normalization • Feature Engineering • Dimensionality Reduction (SVD)
---

## 1. Tổng quan đề tài

Đề tài này xây dựng một pipeline xử lý dữ liệu hoàn chỉnh cho bộ dữ liệu **FIFA23 BIGDATA** nhằm biến dữ liệu cầu thủ từ dạng thô, nhiều chiều và khó quan sát thành một không gian đặc trưng gọn hơn, dễ phân tích hơn và có thể tạo ra output thực tế.

Pipeline của bài gồm 5 phần chính:

1. **Data Profiling**
2. **Data Cleaning**
3. **Data Normalization**
4. **Feature Engineering**
5. **Dimensionality Reduction bằng SVD**

bài này sẽ trả lời rõ:

- bài toán cụ thể là gì
- SVD dùng để làm gì
- output cuối cùng là gì
- kết quả đó giải quyết vấn đề thực tế nào

---

## 2. Bài toán cụ thể của đề tài

### Bài toán được chọn:
**Scouting cầu thủ và tìm cầu thủ tương tự (Player Similarity / Scouting Recommendation)**

Dataset FIFA chứa rất nhiều thông tin về cầu thủ như:
- chỉ số tổng quát
- tiềm năng phát triển
- giá trị chuyển nhượng
- lương
- vị trí thi đấu
- chỉ số kỹ năng
- chỉ số thể chất
- quốc tịch
- CLB
- các thuộc tính mô tả khác

Khi số lượng biến quá lớn, ta gặp các khó khăn sau:

1. rất khó so sánh cầu thủ theo cách tổng quát
2. rất khó nhìn ra nhóm cầu thủ tương đồng
3. rất khó trực quan hóa dữ liệu
4. rất khó xác định cầu thủ thay thế nếu chỉ nhìn từng cột riêng lẻ
5. dữ liệu nhiều chiều làm phân tích trực tiếp trở nên rối và kém hiệu quả

Vì vậy, bài toán được đặt ra là:

> Làm thế nào để làm sạch và chuẩn hóa dữ liệu cầu thủ FIFA, sau đó giảm số chiều dữ liệu bằng SVD để trực quan hóa cấu trúc ẩn và tìm ra những cầu thủ có hồ sơ tương tự nhau phục vụ scouting?

---

## 3. Đề tài này giải quyết cái gì

Đề tài giải quyết đồng thời 3 nhóm vấn đề lớn.

### 3.1. Vấn đề dữ liệu thô
Dữ liệu gốc thường có:
- giá trị thiếu
- dữ liệu trùng
- kiểu dữ liệu chưa chuẩn
- text không đồng nhất
- định dạng tiền tệ chưa sẵn sàng để tính toán
- cột ngày tháng chưa được parse đúng

### 3.2. Vấn đề dữ liệu quá nhiều chiều
Khi dữ liệu có quá nhiều cột:
- khó quan sát trực tiếp
- khó trực quan hóa
- khó xác định biến nào thực sự quan trọng
- khó dùng cho similarity analysis

### 3.3. Vấn đề “SVD để làm gì”
Bài này đi xa hơn bằng cách dùng SVD để:
- giảm chiều dữ liệu
- tạo latent space cho cầu thủ
- trực quan hóa cầu thủ trong không gian mới
- tìm cầu thủ tương tự
- hỗ trợ scouting và đề xuất thay thế cầu thủ

---

## 4. Dataset được sử dụng

### 4.1. Dataset
Dataset sử dụng là **FIFA23 BIGDATA**.

### 4.2. Đặc điểm dữ liệu
Dữ liệu gốc được lưu ở dạng:
- thư mục lớn
- có thể là **Parquet dataset chia nhiều file part**
- không phù hợp để kéo toàn bộ vào pandas trên Colab nếu RAM hạn chế

### 4.3. Kiểu thông tin có trong dataset
Dataset chứa các nhóm biến điển hình như:
- định danh cầu thủ
- vị trí thi đấu
- chỉ số kỹ năng
- chỉ số thể chất
- thông tin giá trị và lương
- thông tin CLB / giải đấu / quốc tịch
- các cột ngày tháng nếu có

---

## 5. Tại sao notebook dùng hai lớp xử lý

Một điểm rất quan trọng trong bài này là notebook không làm theo kiểu kéo toàn bộ dataset vào RAM rồi xử lý một lần, vì cách đó rất dễ làm Colab sập phiên.

Thay vào đó, notebook dùng **hai lớp xử lý**:

### Lớp 1 — Full Dataset Profiling
Toàn bộ dataset được quét theo batch để:
- xác định schema
- đếm tổng số dòng
- đếm số null theo cột
- nhìn được chất lượng dữ liệu ở mức toàn cục

### Lớp 2 — Sample Modeling
Một sample có kiểm soát được lấy ra để thực hiện:
- cleaning chi tiết
- normalization
- feature engineering
- SVD
- visualization
- similarity output

### Ý nghĩa của cách làm này
Cách làm này giúp:
- vẫn giữ được góc nhìn toàn bộ dataset
- vẫn chạy được ổn trên Colab
- tránh nổ RAM
- vẫn đủ để trình bày một pipeline phân tích hoàn chỉnh

---

## 6. Input → Process → Output

### Input
- FIFA23 BIGDATA
- dữ liệu cầu thủ nhiều chiều
- dữ liệu ở dạng parquet dataset hoặc file dữ liệu lớn
- dữ liệu gốc được giữ nguyên, không chỉnh sửa trực tiếp

### Process
1. Dò cấu trúc dataset
2. Đọc schema
3. Profiling toàn dataset
4. Chọn cột quan trọng
5. Đọc sample an toàn RAM
6. Data cleaning
7. Data normalization
8. Feature engineering
9. One-hot encoding + scaling
10. Chạy SVD
11. Visualization
12. Similarity analysis

### Output
- profiling toàn dataset
- sample gốc
- sample cleaned
- sample feature-engineered
- embedding sau SVD
- biểu đồ explained variance
- biểu đồ latent space
- bảng player similarity
- file final checks

---

## 7. Data Cleaning trong bài này làm gì

Phần cleaning giúp biến dữ liệu từ dạng thô sang dạng đủ sạch để dùng cho phân tích.

### Các bước cleaning chính:
- chuẩn hóa tên cột
- chuẩn hóa text
- bỏ dấu và ký tự đặc biệt thừa
- xóa duplicate
- parse currency text nếu có
- parse date thành datetime nếu có
- chuyển object sang numeric khi hợp lý
- loại giá trị âm bất hợp lý
- fill missing:
  - numeric → median
  - categorical → mode

### Tại sao cần cleaning
Nếu không cleaning:
- mô hình dễ bị nhiễu
- one-hot dễ sinh lỗi
- scale không có ý nghĩa
- SVD cho insight kém tin cậy

---

## 8. Data Normalization trong bài này làm gì

Phần normalization giúp đưa dữ liệu về dạng phù hợp cho mô hình.

### Với numeric:
- dùng `StandardScaler`
- đưa các biến số về cùng thang đo

### Với categorical:
- dùng `OneHotEncoder`
- chuyển text thành dạng số

### Vì sao không dùng Label Encoding là chính
Vì nhiều biến categorical trong dữ liệu FIFA:
- không có thứ tự tự nhiên
- nếu ép thành số nguyên bằng label encoding thì dễ tạo ra ý nghĩa thứ tự giả

One-hot an toàn hơn cho bài toán SVD trong trường hợp này.

---

## 9. Feature Engineering trong bài này làm gì

Feature engineering giúp tạo ra thêm những biến có ý nghĩa hơn cho bài toán scouting.

### Các biến được tạo thêm:
- `year`, `month`, `day` từ cột datetime
- `bmi` từ `height_cm` và `weight_kg`
- `potential_gap = potential - overall`
- `value_per_overall = value_eur / overall`
- `position_group` rút gọn từ `player_positions`

### Ý nghĩa
Những biến này giúp:
- tăng giá trị phân tích
- phản ánh tốt hơn hồ sơ cầu thủ
- cải thiện chất lượng latent space
- giúp similarity analysis có ý nghĩa thực tế hơn

---

## 10. Vì sao chọn SVD

SVD được chọn vì dữ liệu sau preprocessing có đặc điểm:
- nhiều chiều
- nhiều biến numeric
- nhiều biến categorical sau one-hot
- cần giảm chiều để dễ phân tích

### SVD được dùng để:
1. giảm số chiều dữ liệu
2. giữ lại phần lớn thông tin quan trọng
3. tạo latent space
4. hỗ trợ visualization
5. hỗ trợ similarity analysis

### Nói ngắn gọn:
SVD giúp biến dữ liệu cầu thủ rất nhiều biến thành một biểu diễn gọn hơn nhưng vẫn giàu thông tin.

---

## 11. Tại sao output là similarity chứ không phải prediction

Bài này chọn output theo hướng:
**insight + recommendation**

chứ không đi theo hướng:
**supervised prediction**

### Lý do:
1. mục tiêu bài toán là hiểu cấu trúc dữ liệu và tìm cầu thủ tương tự
2. SVD phù hợp tự nhiên với exploratory analysis và latent representation
3. similarity/recommendation là output hợp logic hơn cho scouting
4. prediction cần target rõ ràng và bài toán khác hẳn, ví dụ:
   - dự đoán giá trị cầu thủ
   - dự đoán overall
   - dự đoán potential

### Vì vậy:
Bài này chọn output là:
- latent space
- insight từ components
- player similarity recommendation

Đây là lựa chọn đúng logic với đề tài và phù hợp với bản chất của SVD.

---

## 12. Insight kỳ vọng từ kết quả

Sau khi chạy notebook, các insight kỳ vọng gồm:

### 12.1. Explained variance
Cho thấy:
- cần bao nhiêu SVD components để giữ được phần lớn thông tin
- dữ liệu có mức độ nén tốt đến đâu

### 12.2. Latent space
Cho thấy:
- cầu thủ có hồ sơ tương đồng sẽ nằm gần nhau
- có thể xuất hiện các cụm vị trí / phong cách chơi khác nhau
- cầu thủ overall cao có xu hướng phân bố riêng biệt

### 12.3. Top feature importance
Cho thấy:
- latent dimensions chịu ảnh hưởng mạnh bởi những nhóm đặc trưng nào
- ví dụ: kỹ năng tấn công, thể chất, giá trị, tiềm năng...

### 12.4. Player similarity output
Cho thấy:
- với một cầu thủ query, hệ thống có thể tìm ra top cầu thủ gần nhất trong latent space
- đây là output gần với ứng dụng scouting nhất trong bài

---

## 13. Ý nghĩa ứng dụng thực tế

Kết quả của bài có thể ứng dụng theo các hướng sau:

### 13.1. Scouting
Hỗ trợ tuyển trạch viên:
- tìm cầu thủ có hồ sơ tương tự
- tìm phương án thay thế

### 13.2. Phân tích đội hình
Giúp hiểu:
- nhóm cầu thủ nào có phong cách giống nhau
- vị trí nào đang thiếu lựa chọn tương đồng

### 13.3. Trực quan hóa dữ liệu cầu thủ
Thay vì nhìn hàng chục hoặc hàng trăm cột riêng lẻ, có thể nhìn cầu thủ trong latent space 2D/3D.

### 13.4. Nền tảng cho hệ thống recommendation
Embedding sau SVD có thể dùng như đầu vào cho các hệ thống phân tích hoặc recommendation khác.

---

## 14. Giới hạn của bài



### Giới hạn 1
Profiling được thực hiện trên toàn dataset, nhưng:
- cleaning chi tiết
- feature engineering
- normalization
- SVD
- similarity analysis

được thực hiện trên **sample đại diện**, không phải full-data modeling end-to-end.

### Giới hạn 2
Feature importance ở Bước 13 dùng bản nhẹ RAM:
- lưu theo `feature_index`
- không bung toàn bộ tên feature sau one-hot
- nhằm tránh Colab hết RAM

### Giới hạn 3
Bài không làm supervised prediction, nên output được hiểu là:
- insight
- recommendation
- similarity

không phải dự báo nhãn hoặc hồi quy mục tiêu.

---

## 15. Danh sách output chính

Notebook tạo ra các file output quan trọng sau:

- `profiling_full_dataset.csv`
- `schema_columns.json`
- `selected_columns_for_modeling.json`
- `v0_raw_sample_snapshot.csv`
- `v0_raw_sample_snapshot.parquet`
- `profiling_initial_sample.csv`
- `v1_cleaned_sample.csv`
- `v1_cleaned_sample.parquet`
- `v2_feature_engineered_sample.csv`
- `v2_feature_engineered_sample.parquet`
- `svd_explained_variance_sample.csv`
- `v3_model_ready_embedding_sample.csv`
- `v3_model_ready_embedding_sample.parquet`
- `svd_top_feature_importance_sample_light.csv`
- `svd_player_similarity_output_sample.csv`
- `final_checks_sample.json`

---

## 16. Thứ tự chạy notebook

Chạy lần lượt từ:

- Bước 1
- Bước 2
- Bước 3
- ...
- đến Bước 14

### Nếu Colab yếu
Ở Bước 5, giảm:

- `MAX_ROWS = 100000`

xuống:
- `50000`
hoặc
- `30000`

---

## 18. Kết luận

Đề tài này cho thấy SVD có thể được dùng để:

- tổ chức lại dữ liệu cầu thủ nhiều chiều
- trực quan hóa cấu trúc ẩn
- hỗ trợ similarity analysis
- tạo ra output có ý nghĩa thực tế cho scouting

Nói cách khác, bài này biến một bộ dữ liệu cầu thủ lớn và phức tạp thành một hệ thống phân tích gọn hơn, rõ hơn và dùng được hơn.