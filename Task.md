# TASK 1: DATA CLEANING & NORMALIZATION

##  Mục tiêu
- Đầu tiên tìm bộ data phù hợp (yc:>3gb đọc hiểu các trường trong data)
- Làm sạch dữ liệu (Data Cleaning)
- Chuẩn hoá dữ liệu (Data Normalization)
- Chuẩn bị dataset sẵn sàng cho các bước sau

---

##  1. DATA CLEANING 


### Công việc:
- Kiểm tra dữ liệu thiếu (NULL, NaN)
- Xử lý:
  - Xoá dòng thiếu dữ liệu
  - Hoặc fill dữ liệu hợp lý
- Xoá dữ liệu trùng (duplicate)
- Loại bỏ dữ liệu sai:
  - giá trị âm bất hợp lý
  - dữ liệu lỗi format
- Chuẩn hoá kiểu dữ liệu:
  - string → number
  - date → datetime

---

##  2. DATA NORMALIZATION TEAM


### Công việc:
exam:
- Chuẩn hoá dữ liệu số:
  - Scale về cùng khoảng (0–1 hoặc standard)
- Chuẩn hoá dữ liệu dạng text:
  - Viết thường (lowercase)
  - Xoá ký tự đặc biệt
- Encode dữ liệu categorical:
  - Label Encoding / One-hot
- Chuẩn hoá format:
  - ngày tháng
  - tiền tệ

---

##  3. OUTPUT 


### Công việc:
- Kiểm tra lại dữ liệu sau xử lý
- Xuất dữ liệu:
  - định dạng Parquet / CSV
- Đảm bảo:
  - Không còn null
  - Dữ liệu consistent

---

##  Timeline

deline 9h tối

---

##  thứu cân nhắc 
- Không sửa dữ liệu gốc
- Mỗi bước phải lưu lại version
- Code phải có comment rõ ràng

##  Tự phân chia làm đúng hạn là được 
