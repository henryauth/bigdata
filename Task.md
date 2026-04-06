# 📌 PHÂN CÔNG CÔNG VIỆC - BIG DATA FIFA PROJECT

## 🎯 Mục tiêu project
Xây dựng hệ thống phân tích dữ liệu cầu thủ FIFA:
- Xử lý Big Data bằng PySpark
- Phân cụm cầu thủ
- Dự đoán giá trị & rating
- Gợi ý cầu thủ (Recommendation)
- Trực quan hóa dữ liệu

---

## 👥 1. DATA ENGINEER (Xử lý dữ liệu)

### 🔹 Thành viên: [Tên A], [Tên B]

### Công việc:
- Đọc dataset FIFA (CSV lớn)
- Làm sạch dữ liệu:
  - Xóa null
  - Chuẩn hóa format
- Xây pipeline:
  - Bronze (raw data)
  - Silver (clean data)
  - Gold (data cho ML)
- Lưu trữ bằng Parquet

---

## 🤖 2. MACHINE LEARNING

### 🔹 Thành viên: [Tên C], [Tên D]

### Công việc:

#### 📌 Clustering
- Phân cụm cầu thủ theo kỹ năng
- Dùng KMeans / Hierarchical

#### 📌 Prediction
- Dự đoán:
  - Giá trị cầu thủ
  - Overall rating
- Model: Linear Regression / Random Forest

#### 📌 Recommendation (QUAN TRỌNG)
- Gợi ý cầu thủ tương tự
- Dùng:
  - SVD hoặc Cosine Similarity

---

## 📊 3. DATA VISUALIZATION

### 🔹 Thành viên: [Tên E]

### Công việc:
- Vẽ biểu đồ:
  - Phân bố rating
  - Giá trị cầu thủ
  - So sánh các vị trí
- Làm dashboard:
  - Streamlit / Power BI / Tableau

---

## 🧪 4. TEST & DOCUMENT

### 🔹 Thành viên: [Tên F]

### Công việc:
- Test pipeline
- Kiểm tra model
- Viết báo cáo:
  - Mô tả dữ liệu
  - Quy trình xử lý
  - Kết quả đạt được

---

## 📅 Timeline (gợi ý)

| Tuần | Công việc |
|------|---------|
| 1 | Data cleaning + pipeline |
| 2 | Clustering + EDA |
| 3 | Prediction |
| 4 | Recommendation |
| 5 | Visualization + Report |

---

## ⚠️ Quy định làm việc
- Mỗi người update tiến độ mỗi ngày
- Không sửa code của người khác khi chưa báo
- Code phải có comment rõ ràng
