## 1 — DATA CLEANING  
**Bộ dữ liệu:** Amazon Product Reviews Dataset  

### 1. Tổng quan  
Sau khi thu thập dữ liệu, dự án tiến hành bước Data Cleaning nhằm đảm bảo dữ liệu sạch, nhất quán và sẵn sàng cho các bước phân tích tiếp theo.

Dữ liệu ban đầu tồn tại nhiều vấn đề như:
* giá trị thiếu (NULL),
* dữ liệu trùng lặp,
* dữ liệu sai định dạng,
* kiểu dữ liệu chưa phù hợp.

Nếu không xử lý, các vấn đề này có thể gây sai lệch kết quả phân tích hoặc làm giảm hiệu quả của mô hình học máy.

**Mục tiêu của bước này gồm:**
* xử lý dữ liệu thiếu,
* loại bỏ dữ liệu trùng,
* loại bỏ dữ liệu sai,
* chuẩn hoá kiểu dữ liệu,
* giữ nguyên dữ liệu gốc,
* tạo dataset sạch cho các bước tiếp theo.

---

### 2. Nội dung thực hiện  

---

#### **Xử lý dữ liệu thiếu (NULL)**

Dữ liệu được kiểm tra để phát hiện các giá trị NULL, đặc biệt ở các cột quan trọng như:
* `reviews_rating`
* `reviews_text`

Đây là các thuộc tính chính phục vụ cho phân tích đánh giá. Nếu thiếu các giá trị này, bản ghi sẽ không còn ý nghĩa.

**Phương pháp xử lý:**
* Không sử dụng phương pháp điền giá trị (fill) do khó xác định giá trị thay thế hợp lý
* Loại bỏ các dòng có giá trị NULL ở các cột quan trọng

**Kết quả:**
* Dataset chỉ giữ lại các bản ghi đầy đủ thông tin cần thiết

---

#### **Xóa dữ liệu trùng lặp (Duplicate Removal)**

Trong dataset tồn tại các bản ghi trùng lặp hoàn toàn, có thể phát sinh trong quá trình thu thập dữ liệu.

**Phương pháp xử lý:**
* Loại bỏ các dòng trùng lặp trên toàn bộ dataset

**Kết quả:**
* Mỗi bản ghi là duy nhất
* Giảm nhiễu và sai lệch trong phân tích

---

#### **Loại bỏ dữ liệu sai hoặc bất hợp lý**

Một số giá trị trong dataset không hợp lệ hoặc sai định dạng, bao gồm:
* giá trị không thể chuyển sang dạng số,
* dữ liệu chứa ký tự không hợp lệ,
* dữ liệu lỗi định dạng ngày.

**Phương pháp xử lý:**
* Sử dụng phương pháp chuyển đổi an toàn để phát hiện lỗi
* Các giá trị lỗi sẽ được chuyển thành NULL
* Sau đó loại bỏ các bản ghi chứa giá trị không hợp lệ

**Kết quả:**
* Dataset chỉ chứa dữ liệu hợp lệ và có thể sử dụng

---

#### **Chuẩn hoá kiểu dữ liệu (Data Type Standardization)**

Dữ liệu ban đầu có nhiều cột lưu dưới dạng string nhưng thực chất là dữ liệu số hoặc thời gian. Điều này gây khó khăn cho việc phân tích và tính toán.

---

##### **Chuyển đổi String → Numeric**

Cột `reviews_rating` ban đầu ở dạng string, trong khi đây là dữ liệu số.

Thay vì ghi đè trực tiếp, dự án chuyển đổi theo hướng:

> `reviews_rating (string) → rating_numeric (double)`

---

##### **Giải thích lựa chọn**

Việc tạo cột mới thay vì ghi đè được lựa chọn vì:

* **Giữ nguyên dữ liệu gốc**  
  → phục vụ kiểm tra và đối chiếu khi cần

* **Tránh mất dữ liệu khi có lỗi format**  
  → các giá trị lỗi sẽ được chuyển thành NULL thay vì làm hỏng toàn bộ pipeline

* **Dễ debug**  
  → có thể so sánh dữ liệu trước và sau khi xử lý

* **Phù hợp với quy trình xử lý dữ liệu nhiều tầng (data pipeline)**  
  → tách biệt dữ liệu thô và dữ liệu đã xử lý

---

##### **Chuyển đổi String → Datetime**

Cột `reviews_date` được chuyển từ string sang datetime để phục vụ phân tích theo thời gian.

**Kết quả:**
* Dữ liệu thời gian có thể sử dụng cho các phân tích như xu hướng, thống kê theo ngày/tháng

---

### 3. Kết luận  

Sau bước Data Cleaning:

* Dữ liệu thiếu đã được loại bỏ
* Dữ liệu trùng đã được xử lý
* Dữ liệu sai và lỗi format đã được loại bỏ
* Kiểu dữ liệu đã được chuẩn hoá:
  * `reviews_rating` → `rating_numeric`
  * `reviews_date` → datetime
* Dữ liệu gốc được giữ nguyên
