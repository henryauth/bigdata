 
# FIFA23 BIGDATA LAKEHOUSE PROJECT  
## Xây dựng hệ thống Lakehouse và Machine Learning cho phân tích cầu thủ FIFA23

---

## 1. Tổng quan đề tài

Đồ án này xây dựng một hệ thống xử lý dữ liệu theo kiến trúc **Lakehouse** kết hợp với **Machine Learning** trên bộ dữ liệu **FIFA23 BIGDATA**.  
 đề tài tổ chức dữ liệu thành nhiều tầng và tạo ra các đầu ra phục vụ phân tích, dự đoán và hỗ trợ ra quyết định.

Hệ thống được chia thành nhiều tầng dữ liệu:

- **BRONZE**: dữ liệu gốc
- **SILVER**: dữ liệu đã làm sạch và chuẩn hóa
- **GOLD**: dữ liệu đầu ra phục vụ phân tích và machine learning
- **MODELS**: nơi lưu mô hình đã huấn luyện

Đây là cách tiếp cận đúng tinh thần Big Data hiện đại:  
không xử lý dữ liệu một cách rời rạc, mà tổ chức toàn bộ pipeline theo cấu trúc rõ ràng, có khả năng mở rộng và tái sử dụng.

---

## 2. Bài toán cụ thể của đề tài

### Bài toán trung tâm
Đề tài tập trung vào bài toán:

# **Phân tích và mô hình hóa hồ sơ cầu thủ FIFA để hỗ trợ scouting, dự đoán và phân cụm cầu thủ**

Cụ thể, bài toán được chia thành các nhánh chính:

### 2.1. Bài toán dự đoán giá trị cầu thủ
Dựa trên các đặc trưng của cầu thủ như:
- chỉ số chuyên môn
- thể chất
- tuổi
- tiềm năng
- lương
- vị trí
- quốc tịch / câu lạc bộ

hệ thống sẽ dự đoán:
- **`value_eur`**: giá trị của cầu thủ

### 2.2. Bài toán dự đoán chỉ số tổng quát
Hệ thống dự đoán:
- **`overall`**: chỉ số tổng quát của cầu thủ

Điều này giúp đánh giá xem các đặc trưng hiện có phản ánh chất lượng cầu thủ tốt tới đâu.

### 2.3. Bài toán phân cụm cầu thủ
Sử dụng các đặc trưng chính để:
- nhóm cầu thủ thành các cụm tương đồng
- khám phá cấu trúc ẩn của dữ liệu
- hỗ trợ phân loại kiểu cầu thủ

### 2.4. Bài toán học biểu diễn và tìm cầu thủ tương tự
Sử dụng **SVD** để:
- giảm chiều dữ liệu
- học biểu diễn tiềm ẩn của cầu thủ
- tìm các cầu thủ có hồ sơ gần giống nhau

Đây là bài toán rất phù hợp với scouting vì thay vì nhìn từng cột riêng lẻ, ta có thể nhìn cầu thủ trong một không gian đặc trưng gọn hơn.

---

## 3. Bài toán này giải quyết cái gì

Đề tài giải quyết đồng thời nhiều vấn đề thực tế trong xử lý dữ liệu cầu thủ.

### 3.1. Giải quyết vấn đề dữ liệu lớn và khó tổ chức
Dữ liệu FIFA23 có:
- kích thước lớn
- nhiều cột
- kiểu dữ liệu hỗn hợp
- khó xử lý nếu chỉ dùng một file đơn

Giải pháp:
- tổ chức theo kiến trúc **Bronze – Silver – Gold – Models**

### 3.2. Giải quyết vấn đề dữ liệu thô, chưa sẵn sàng cho mô hình
Dữ liệu gốc thường:
- có missing values
- kiểu dữ liệu chưa chuẩn
- text chưa đồng nhất
- có thể có dữ liệu không hợp lệ

Giải pháp:
- cleaning
- casting
- fillna
- feature engineering
- normalization

### 3.3. Giải quyết vấn đề khó phân tích cầu thủ theo nhiều chiều
Mỗi cầu thủ có rất nhiều chỉ số, nên:
- khó trực quan hóa
- khó so sánh
- khó tìm cầu thủ tương tự

Giải pháp:
- dùng **SVD** để giảm chiều và tạo latent representation

### 3.4. Giải quyết vấn đề thiếu output thực tế
Nhiều bài chỉ dừng ở chỗ train model, nhưng không tạo ra output có ích.

Giải pháp:
- dự đoán giá trị cầu thủ
- dự đoán overall
- phân cụm
- tìm cầu thủ tương tự
- tạo các bảng Gold rõ ràng phục vụ phân tích

---

## 4. Mục tiêu của đồ án

### 4.1. Mục tiêu tổng quát
Xây dựng một hệ thống dữ liệu cầu thủ theo kiến trúc Lakehouse kết hợp các kỹ thuật Machine Learning để tạo ra những đầu ra phục vụ phân tích, dự đoán và scouting.

### 4.2. Mục tiêu cụ thể
1. Đọc dữ liệu cầu thủ từ nguồn lớn `male_players.csv`
2. Tạo tầng Bronze cho dữ liệu gốc
3. Tạo tầng Silver cho dữ liệu đã làm sạch và chuẩn hóa
4. Tạo tầng Gold cho các bảng phân tích và machine learning
5. Huấn luyện các mô hình dự đoán giá trị và overall
6. Phân cụm cầu thủ
7. Sử dụng SVD để học biểu diễn và phục vụ similarity retrieval
8. Tạo output rõ ràng theo đúng cấu trúc thư mục dữ liệu
9. Giúp đồ án không chỉ là “chạy mô hình”, mà là một hệ thống dữ liệu có cấu trúc

---

## 5. Dataset sử dụng

### 5.1. Nguồn dữ liệu chính
Đồ án sử dụng file:
- **`male_players.csv`**

Đây là bảng dữ liệu chính chứa hồ sơ chi tiết của các cầu thủ.

### 5.2. Đặc điểm dữ liệu
Dữ liệu bao gồm:
- thông tin định danh cầu thủ
- vị trí thi đấu
- tuổi
- chỉ số tổng quát và tiềm năng
- giá trị cầu thủ và lương
- các chỉ số kỹ thuật
- các chỉ số thể chất
- các chỉ số mô tả kỹ năng chơi bóng

### 5.3. Tính chất Big Data
Dữ liệu có kích thước lớn, vì vậy:
- không phù hợp để xử lý ngây thơ bằng pandas ở mọi bước
- cần chia tầng dữ liệu
- cần có tư duy pipeline
- cần có chiến lược sample hợp lý cho modeling trong Colab

---

## 6. Kiến trúc Lakehouse của đồ án

### 6.1. BRONZE
Tầng này chứa dữ liệu gốc gần với nguồn nhất.

**Trong đồ án:**
- `BRONZE/players`

**Vai trò:**
- ingest dữ liệu từ file nguồn
- giữ raw data
- làm điểm bắt đầu cho toàn bộ pipeline

### 6.2. SILVER
Tầng này chứa dữ liệu đã được làm sạch, ép kiểu và bổ sung một số đặc trưng.

**Trong đồ án:**
- `SILVER/players_silver`

**Vai trò:**
- loại bỏ dữ liệu trùng
- chuẩn hóa dữ liệu
- xử lý dữ liệu thiếu
- tạo feature cơ bản
- chuẩn bị dữ liệu cho phân tích và machine learning

### 6.3. GOLD
Tầng này chứa output phục vụ phân tích cuối cùng.

**Trong đồ án:**
- `GOLD/gold_player_stats`
- `GOLD/gold_value_ml`
- `GOLD/gold_overall_ml`
- `GOLD/gold_cluster_result`
- `GOLD/gold_cluster_ml`

**Vai trò:**
- lưu kết quả thống kê
- lưu bảng dự đoán
- lưu bảng phân cụm
- lưu output cho use case cuối cùng

### 6.4. MODELS
Tầng này chứa mô hình đã huấn luyện.

**Trong đồ án:**
- `MODELS/value_predict_rf`

**Vai trò:**
- lưu artifact của model
- có thể dùng lại về sau
- đúng tinh thần một hệ thống ML có tổ chức

---

## 7. Feature Engineering trong đề tài

Feature engineering là bước rất quan trọng vì dữ liệu gốc không phải lúc nào cũng đủ biểu đạt vấn đề tốt nhất.

### Các feature được tạo thêm

### 7.1. `position_group`
Từ `player_positions`, cầu thủ được gom vào các nhóm:
- forward
- midfielder
- defender
- goalkeeper
- other

**Ý nghĩa:**  
Giúp đơn giản hóa không gian vị trí và hỗ trợ:
- phân tích
- phân cụm
- similarity
- evaluation

### 7.2. `bmi`
Được tính từ:
- `height_cm`
- `weight_kg`

**Ý nghĩa:**  
Thêm thông tin về thể trạng cầu thủ.

### 7.3. `potential_gap`
Được tính bằng:
- `potential - overall`

**Ý nghĩa:**  
Phản ánh khoảng cách giữa trình độ hiện tại và tiềm năng phát triển.

### Vì sao feature engineering cần thiết
Nếu chỉ giữ dữ liệu gốc, mô hình có thể:
- thiếu ngữ cảnh
- khó học các mối quan hệ tốt hơn
- khó phản ánh các đặc điểm ẩn

Feature engineering giúp dữ liệu có tính biểu đạt tốt hơn.

---

## 8. Data Cleaning và Data Normalization trong đồ án

### 8.1. Data Cleaning
Ở tầng Silver, dữ liệu được làm sạch theo các bước chính:
- chọn cột quan trọng
- ép kiểu numeric
- xử lý dữ liệu thiếu
- fill giá trị hợp lý
- loại duplicate
- chuẩn hóa text ở mức cần thiết

### 8.2. Data Normalization
Ở bước modeling:
- biến số được scale bằng `StandardScaler`
- biến phân loại được encode bằng `OneHotEncoder`

### Vì sao cần chuẩn hóa
Nếu không chuẩn hóa:
- các biến có thang đo lớn sẽ lấn át các biến khác
- similarity và SVD sẽ kém ổn định
- mô hình dự đoán khó đạt hiệu quả tốt hơn

---

## 9. SVD trong đồ án này để làm gì

Đây là phần rất quan trọng, vì nhiều bài chỉ nói “dùng SVD” nhưng không giải thích rõ mục đích.

## SVD được dùng để làm gì?
SVD được dùng để:

1. **Giảm chiều dữ liệu**
   - Dữ liệu cầu thủ có rất nhiều biến
   - Sau khi encode, số chiều tăng rất mạnh
   - SVD giúp nén dữ liệu về một số chiều nhỏ hơn

2. **Giữ lại phần lớn thông tin quan trọng**
   - Thay vì giữ toàn bộ hàng trăm hoặc hàng nghìn chiều
   - SVD giữ lại phần thông tin có ích nhất trong latent space

3. **Tạo latent representation cho cầu thủ**
   - Mỗi cầu thủ được biểu diễn bằng vector đặc trưng gọn hơn
   - Vector này phản ánh hồ sơ cầu thủ ở mức khái quát hơn

4. **Phục vụ similarity retrieval**
   - Sau khi có embedding SVD, ta có thể tính cosine similarity
   - Từ đó tìm các cầu thủ tương tự nhau

5. **Phục vụ visualization**
   - Có thể biểu diễn dữ liệu trên không gian 2D hoặc 3D dễ nhìn hơn
   - Giúp quan sát cụm cầu thủ và phân bố của dữ liệu

## Nói ngắn gọn:
> SVD trong đồ án này được dùng để học biểu diễn cầu thủ trong không gian đặc trưng thấp chiều hơn, giúp trực quan hóa dữ liệu và tìm cầu thủ tương tự phục vụ scouting.

---

## 10. Visualization trong đồ án

Visualization không chỉ để cho đẹp, mà phục vụ giải thích mô hình và hiểu dữ liệu.

### Các visualization chính

### 10.1. Biểu đồ cumulative explained variance
Cho thấy:
- khi tăng số chiều SVD thì lượng thông tin giữ lại tăng như thế nào
- bao nhiêu components là hợp lý

### 10.2. Biểu đồ latent space 2D
Hiển thị cầu thủ trong không gian sau giảm chiều:
- trục `SVD 1`
- trục `SVD 2`

**Ý nghĩa:**
- thấy các cụm cầu thủ
- thấy phân bố theo overall hoặc nhóm vị trí
- hỗ trợ giải thích trực quan

### 10.3. Bảng phân cụm
Dù không phải biểu đồ, nhưng cụm cầu thủ trung bình theo nhóm cũng là một dạng visualization dạng bảng, rất hữu ích cho báo cáo.

---

## 11. Các mô hình trong đồ án

### 11.1. Random Forest Regressor cho `value_eur`
**Bài toán:** hồi quy  
**Mục tiêu:** dự đoán giá trị cầu thủ

**Ý nghĩa:**
- xem liệu hồ sơ cầu thủ có phản ánh giá trị kinh tế hay không
- tạo ra bảng dự đoán giá trị ở Gold

**Output:**
- prediction table
- metrics: MAE, RMSE, R²
- model artifact trong MODELS

### 11.2. Random Forest Regressor cho `overall`
**Bài toán:** hồi quy  
**Mục tiêu:** dự đoán chỉ số tổng quát của cầu thủ

**Ý nghĩa:**
- kiểm tra mức độ các feature có thể giải thích chất lượng tổng thể

**Output:**
- prediction table
- metrics: MAE, RMSE, R²

### 11.3. KMeans Clustering
**Bài toán:** phân cụm  
**Mục tiêu:** nhóm cầu thủ thành các cluster có đặc điểm tương đồng

**Ý nghĩa:**
- khám phá cấu trúc ẩn của dữ liệu
- nhận diện kiểu cầu thủ
- phục vụ scouting và segmentation

**Output:**
- bảng cluster result
- bảng cluster summary

### 11.4. Truncated SVD + Similarity Retrieval
**Bài toán:** representation learning + retrieval  
**Mục tiêu:** học embedding và tìm cầu thủ tương tự

**Ý nghĩa:**
- hỗ trợ scouting
- hỗ trợ thay thế cầu thủ
- tạo insight từ latent space

**Output:**
- embedding
- top similar players
- evaluation retrieval

---

## 12. Output của đồ án

## 12.1. Output dạng prediction
### Prediction 1: giá trị cầu thủ
- đầu ra: `gold_value_ml`
- nội dung:
  - actual_value_eur
  - predicted_value_eur

### Prediction 2: overall
- đầu ra: `gold_overall_ml`
- nội dung:
  - actual_overall
  - predicted_overall

## 12.2. Output dạng insight
### Insight 1: thống kê cầu thủ theo nhóm
- đầu ra: `gold_player_stats`

### Insight 2: cấu trúc cluster cầu thủ
- đầu ra:
  - `gold_cluster_result`
  - `gold_cluster_ml`

### Insight 3: cầu thủ tương tự trong latent space
- đầu ra từ SVD + similarity

### Insight 4: explained variance của SVD
- cho biết mô hình giữ được bao nhiêu thông tin

---

## 13. Insight có thể rút ra từ đề tài

### Insight 1
Các chỉ số kỹ thuật và thể chất có thể dùng để dự đoán khá tốt giá trị cầu thủ và overall.

### Insight 2
Một số nhóm vị trí có hồ sơ đặc trưng khá rõ, thể hiện qua clustering và similarity retrieval.

### Insight 3
SVD giúp nén dữ liệu nhiều chiều về một latent space gọn hơn mà vẫn giữ phần lớn thông tin quan trọng.

### Insight 4
Trong latent space, cầu thủ cùng vai trò hoặc hồ sơ tương đồng có xu hướng gần nhau hơn, hỗ trợ tốt cho bài toán scouting.

### Insight 5
Kiến trúc Bronze – Silver – Gold giúp tách biệt rõ vai trò của từng loại dữ liệu:
- dữ liệu gốc
- dữ liệu đã xử lý
- dữ liệu dùng cho phân tích và machine learning

---

## 14. Đánh giá mô hình

### 14.1. Với hồi quy
Các metric được dùng:
- MAE
- RMSE
- R²

### 14.2. Với clustering
Đồ án tập trung vào:
- bảng mô tả cluster
- phân tích trung bình các chỉ số trong từng cluster

### 14.3. Với similarity/SVD
Có thể đánh giá bằng:
- Precision@K theo `position_group`
- độ hợp lý của top similar players
- explained variance

---

## 15. Ý nghĩa ứng dụng thực tế

### 15.1. Scouting
- tìm cầu thủ tương tự
- hỗ trợ thay thế cầu thủ
- lọc cầu thủ theo cụm đặc trưng

### 15.2. Phân tích cầu thủ
- phân tích theo vị trí
- phân tích theo nhóm kỹ năng
- phân tích theo giá trị và tiềm năng

### 15.3. Hệ thống dữ liệu
- làm nền cho dashboard
- làm nền cho API
- làm nền cho recommender system

### 15.4. Giáo dục và nghiên cứu
- minh họa kiến trúc Lakehouse
- minh họa pipeline ML trên dữ liệu lớn
- minh họa mối liên hệ giữa dữ liệu thô và output nghiệp vụ

---

## 16. Điểm mạnh của đồ án

1. Có bài toán cụ thể rõ ràng.
2. Có kiến trúc dữ liệu theo hướng Lakehouse.
3. Có đầy đủ Bronze / Silver / Gold / Models.
4. Có feature engineering rõ ràng.
5. Có nhiều mô hình thay vì chỉ một mô hình đơn lẻ.
6. Có prediction và insight.
7. Có SVD phục vụ representation learning và similarity.
8. Có visualization.
9. Có output cụ thể, dễ trình bày.
10. Có thể mở rộng tiếp thành dashboard, API hoặc hệ gợi ý.

---

---

## 17. Hướng phát triển tiếp theo

1. Tích hợp thêm dữ liệu đội bóng và giải đấu.
2. Xây dựng dashboard trực quan hóa cầu thủ.
3. Xây dựng hệ gợi ý cầu thủ tương tự hoàn chỉnh.
4. Tích hợp orchestration pipeline.
5. Dùng Delta Lake đầy đủ hơn với versioning/time travel.
6. So sánh thêm với XGBoost, LightGBM hoặc deep learning.
7. Đưa hệ thống lên môi trường cloud.

---

## 18. Kết luận

Đồ án này không chỉ là một bài machine learning đơn lẻ, mà là một hệ thống dữ liệu có tổ chức, trong đó:

- dữ liệu được ingest vào Bronze
- được làm sạch và chuẩn hóa ở Silver
- được chuyển hóa thành các bảng phân tích và machine learning ở Gold
- mô hình được lưu ở Models

Về mặt bài toán, đồ án giải quyết rõ ràng các mục tiêu:
- dự đoán giá trị cầu thủ
- dự đoán overall
- phân cụm cầu thủ
- học biểu diễn bằng SVD
- tìm cầu thủ tương tự

Về mặt kỹ thuật, đồ án thể hiện sự kết hợp của:
- Big Data mindset
- Lakehouse architecture
- feature engineering
- machine learning
- visualization
- insight generation

Nói cách khác, đây là một đồ án có thể xem như bước trung gian giữa:
- một bài xử lý dữ liệu lớn
- và một hệ thống phân tích cầu thủ có khả năng phát triển thành sản phẩm thực tế.

---

## 20. Cấu trúc file đầu ra

```text
FIFA23_BIGDATA/
├── BRONZE/
│   └── players/
│       ├── _delta_log/
│       └── part-*.snappy.parquet
│
├── SILVER/
│   └── players_silver/
│       ├── _delta_log/
│       └── part-*.snappy.parquet
│
├── GOLD/
│   ├── gold_player_stats/
│   │   ├── _delta_log/
│   │   └── part-*.snappy.parquet
│   ├── gold_value_ml/
│   │   ├── _delta_log/
│   │   └── part-*.snappy.parquet
│   ├── gold_overall_ml/
│   │   ├── _delta_log/
│   │   └── part-*.snappy.parquet
│   ├── gold_cluster_result/
│   │   ├── _delta_log/
│   │   └── part-*.snappy.parquet
│   └── gold_cluster_ml/
│       ├── _delta_log/
│       └── part-*.snappy.parquet
│
└── MODELS/
    └── value_predict_rf/
        ├── metadata/
        ├── data/
        └── treesMetadata/
```

---
