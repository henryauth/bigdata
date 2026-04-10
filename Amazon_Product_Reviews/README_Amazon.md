

## 1. Mở đầu

đề tài **Dimensionality Reduction: SVD**, áp dụng trên **Amazon Product Reviews Dataset**.

 dữ liệu ngày càng lớn không chỉ về số lượng bản ghi mà còn lớn về số chiều đặc trưng. Khi số chiều quá cao, dữ liệu sẽ gặp nhiều vấn đề như:

- tốn bộ nhớ lưu trữ
- chi phí tính toán cao
- thời gian xử lý lâu
- dữ liệu bị thưa
- và khó khai thác được cấu trúc quan trọng bên trong

Vì vậy, một hướng tiếp cận rất quan trọng là **giảm chiều dữ liệu**.  
Trong đề tài này,sử dụng **SVD**, cụ thể hơn là **Truncated SVD**, để giảm số chiều của dữ liệu review sản phẩm từ Amazon sau khi đã làm sạch và chuẩn hóa.

## 2. Mục tiêu đề tài

Đề tài của có 2 mục tiêu chính.

### Mục tiêu thứ nhất là Task 

Thực hiện **Data Cleaning** và **Data Normalization**, gồm:

- chọn dataset phù hợp
- kiểm tra và xử lý dữ liệu thiếu
- xóa dữ liệu trùng lặp
- chuẩn hóa định dạng
- chuẩn hóa dữ liệu text
- chuẩn hóa dữ liệu số
- và tạo ra dataset sẵn sàng cho bước giảm chiều

### Mục tiêu thứ hai là Task 

Áp dụng **Dimensionality Reduction bằng SVD** để:

- biến dữ liệu có số chiều lớn thành dữ liệu có số chiều thấp hơn
- nhưng vẫn giữ lại phần thông tin quan trọng nhất
- từ đó giúp dữ liệu dễ xử lý và hiệu quả hơn trong các bước phân tích tiếp theo

## 3. Vì sao chọn Amazon Product Reviews Dataset

 chọn **Amazon Product Reviews Dataset** vì đây là bộ dữ liệu rất phù hợp với đề tài SVD.

Lý do là:

- dữ liệu có quy mô lớn
- chứa review text, rating, helpfulness, thời gian đánh giá
- đặc biệt phần review text khi vector hóa sẽ tạo ra ma trận rất lớn và rất thưa

Đây chính là kiểu dữ liệu mà **Truncated SVD** hoạt động rất hiệu quả.

Nếu dùng dữ liệu tabular thuần như bảng số thông thường thì vẫn làm được giảm chiều, nhưng sẽ không thể hiện rõ lợi thế của SVD bằng dữ liệu văn bản lớn.  
Còn với Amazon reviews, sau khi đưa text qua **TF-IDF**, số lượng đặc trưng tăng mạnh theo số từ vựng, nên bài toán giảm chiều trở nên rất có ý nghĩa.

## 4. Cơ sở lý thuyết: Dimensionality Reduction là gì

**Dimensionality Reduction** là quá trình giảm số lượng đặc trưng của dữ liệu.

Mục tiêu không phải là xóa dữ liệu một cách ngẫu nhiên, mà là:

- giữ lại phần thông tin quan trọng nhất
- loại bỏ phần ít quan trọng hoặc nhiễu
- làm dữ liệu gọn hơn nhưng vẫn hữu ích

Ví dụ: nếu dữ liệu ban đầu có hàng nghìn hay hàng chục nghìn đặc trưng, ta có thể giảm xuống còn vài chục hoặc vài trăm đặc trưng đại diện hơn.

Lợi ích của giảm chiều gồm:

- giảm chi phí tính toán
- giảm dung lượng dữ liệu
- tăng tốc mô hình
- giảm nhiễu
- và trong một số trường hợp còn giúp phát hiện cấu trúc tiềm ẩn của dữ liệu

## 5. SVD là gì

**SVD** là viết tắt của **Singular Value Decomposition**, tức là **phân rã giá trị suy biến**.

Về mặt toán học, một ma trận dữ liệu **A** có thể được phân rã thành:

```text
A = UΣVᵀ
```

Trong đó:

- **U** là ma trận chứa các hướng đặc trưng bên trái
- **Σ** là ma trận đường chéo chứa các singular values
- **Vᵀ** là ma trận chứa các hướng đặc trưng bên phải

Điểm quan trọng nhất ở đây là:

- các singular values cho biết mức độ quan trọng của từng thành phần
- singular value càng lớn thì thành phần đó càng mang nhiều thông tin

Từ đó, thay vì giữ toàn bộ dữ liệu ban đầu, ta chỉ giữ lại **k singular values lớn nhất** và các vector tương ứng. Khi đó, dữ liệu được xấp xỉ bởi:

```text
Aₖ = UₖΣₖVₖᵀ
```

Ý nghĩa là:

- dữ liệu được biểu diễn trong không gian ít chiều hơn
- nhưng vẫn giữ lại phần lớn thông tin chính

Nói ngắn gọn:  
**SVD giúp nén dữ liệu theo cách thông minh, giữ phần quan trọng và bỏ phần kém quan trọng hơn.**

## 6. Vì sao dùng Truncated SVD thay vì Full SVD

Trong lý thuyết, có thể dùng **full SVD** cho toàn bộ ma trận.  
Tuy nhiên, trong Big Data thì cách đó thường rất nặng vì:

- dữ liệu lớn
- số chiều cao
- ma trận thường sparse
- chi phí tính toán cao

Vì vậy trong thực tế, người ta hay dùng **Truncated SVD**.

**Truncated SVD** chỉ giữ lại một số thành phần quan trọng nhất thay vì phân rã toàn bộ ma trận.  
Cách này có 3 ưu điểm lớn:

- giảm thời gian tính toán
- phù hợp với ma trận thưa như TF-IDF
- và vẫn đáp ứng đúng mục tiêu giảm chiều

Đó là lý do trong đề tài này  dùng **Truncated SVD** thay vì full SVD.

## 7. Quy trình thực hiện đề tài


# PHẦN  1: DATA CLEANING & NORMALIZATION

## 8. Đọc dữ liệu và hiểu cấu trúc dữ liệu

Ở bước đầu tiên,  đọc dataset Amazon Product Reviews và xác định các trường quan trọng cần dùng, ví dụ:

- nội dung review
- rating
- số lượt đánh giá hữu ích
- thời gian review
- và một số trường liên quan khác nếu có

Mục tiêu của bước này là:

- hiểu dữ liệu
- biết cột nào cần giữ
- cột nào không cần thiết
- và chuẩn bị cho các bước làm sạch phía sau

## 9. Kiểm tra và xử lý dữ liệu thiếu

Sau khi đọc dữ liệu,  kiểm tra các giá trị thiếu như:

- NULL
- NaN
- chuỗi rỗng
- hoặc text không hợp lệ

Việc xử lý dữ liệu thiếu là rất quan trọng vì nếu còn missing value, các bước xử lý sau như vector hóa, chuẩn hóa hoặc SVD có thể lỗi hoặc cho kết quả không ổn định.

Tùy từng cột, áp dụng một trong các hướng:

- loại bỏ bản ghi thiếu dữ liệu quan trọng
- hoặc thay thế bằng giá trị hợp lý nếu cần

Mục tiêu là đảm bảo dữ liệu đầu vào sạch và nhất quán hơn.

## 10. Xóa dữ liệu trùng lặp

Tiếp theo, kiểm tra và loại bỏ các dòng trùng lặp.

Lý do cần làm bước này là vì dữ liệu trùng:

- làm tăng kích thước dataset không cần thiết
- gây thiên lệch phân bố dữ liệu
- và ảnh hưởng đến kết quả phân tích

Với dữ liệu review, nếu cùng một nội dung bị lặp lại nhiều lần một cách bất thường, điều đó có thể làm tăng sai lệch khi xây dựng ma trận đặc trưng.

## 11. Chuẩn hóa kiểu dữ liệu

Sau đó chuẩn hóa kiểu dữ liệu.

Ví dụ:

- rating được chuyển sang kiểu số
- date được chuyển sang kiểu thời gian chuẩn
- số lượt hữu ích được chuyển về dạng numeric

Bước này giúp dữ liệu:

- đồng nhất về định dạng
- dễ xử lý hơn trong các bước feature engineering
- và tránh lỗi khi tính toán

## 12. Chuẩn hóa dữ liệu text

Đây là bước rất quan trọng vì bài  dùng review text để tạo ma trận TF-IDF.

Ở bước này, thực hiện:

- chuyển toàn bộ text về chữ thường
- loại bỏ ký tự đặc biệt
- loại bỏ khoảng trắng thừa
- và chuẩn hóa nội dung review thành dạng gọn, nhất quán

Mục đích của bước này là:

- giảm nhiễu trong văn bản
- tránh trường hợp cùng một từ nhưng bị xem là khác nhau chỉ vì khác hoa/thường hoặc ký tự lạ
- làm cho bước TF-IDF phía sau chính xác hơn

## 13. Tạo thêm đặc trưng phục vụ SVD

Ngoài text, còn tạo thêm một số đặc trưng số như:

- rating_numeric
- reviews_numHelpful
- review_year
- review_month
- review_length_chars

Các đặc trưng này giúp bài toán không chỉ dựa vào text mà còn kết hợp thêm thông tin hành vi và thời gian.  
Sau đó, các đặc trưng số này được scale để đưa về cùng mặt bằng xử lý.

Điểm quan trọng là:  
**Task  không chỉ dừng ở cleaning, mà còn chuẩn bị trực tiếp cho việc tạo feature matrix đầu vào của SVD.**

## 14.  Lưu version theo từng bước

Một yêu cầu của bài là:

- không sửa dữ liệu gốc
- mỗi bước phải lưu lại version

Vì vậy trong notebook, lưu dữ liệu theo từng phiên bản như:

- raw version
- cleaned version
- normalized version
- và version sẵn sàng cho SVD

Việc này giúp:

- dễ kiểm tra lại từng bước
- có thể tái lập quy trình
- và thuận lợi khi báo cáo hoặc debug lỗi

# PHẦN 2: DIMENSIONALITY REDUCTION USING SVD

## 15. Tạo TF-IDF matrix

Sau khi làm sạch và chuẩn hóa review text,  chuyển dữ liệu văn bản sang dạng số bằng **TF-IDF**.

TF-IDF là phương pháp biểu diễn văn bản thành vector số dựa trên:

- tần suất xuất hiện của từ trong tài liệu
- và mức độ đặc trưng của từ trong toàn bộ tập dữ liệu

Sau bước này:

- mỗi review được biểu diễn thành một vector
- toàn bộ tập review trở thành một ma trận đặc trưng rất lớn
- và ma trận này thường rất thưa, vì mỗi review chỉ chứa một phần nhỏ trong toàn bộ từ vựng

Đây chính là đầu vào lý tưởng cho **Truncated SVD**.

## 16.  Ghép thêm đặc trưng số

Sau khi có TF-IDF matrix từ review text,  ghép thêm các đặc trưng số như:

- rating
- helpful votes
- year
- month
- review length

Việc ghép này giúp dữ liệu đầu vào phản ánh không chỉ nội dung văn bản mà còn thêm thông tin định lượng hỗ trợ phân tích.

Kết quả là em thu được một **feature matrix** hoàn chỉnh bao gồm:

- text features
- numeric features

Đây là ma trận đầu vào cuối cùng cho bước giảm chiều.

## 17.  Áp dụng Truncated SVD

Từ feature matrix này,  áp dụng **TruncatedSVD** để giảm chiều.

Nguyên tắc là:

- giữ lại một số thành phần quan trọng nhất
- loại bỏ những thành phần ít đóng góp hơn

Kết quả sau SVD là:

- số chiều dữ liệu giảm xuống
- dữ liệu gọn hơn
- dễ xử lý hơn
- nhưng vẫn giữ lại phần lớn thông tin quan trọng

Nếu nói dễ hiểu, thì:  
thay vì dùng hàng nghìn đặc trưng ban đầu, em biểu diễn mỗi review bằng một số lượng nhỏ hơn các “thành phần ẩn” nhưng mang nhiều ý nghĩa hơn.

## 18. Explained Variance có ý nghĩa gì

Sau khi chạy SVD, theo dõi chỉ số **explained variance ratio**.

Chỉ số này cho biết:

- mỗi thành phần SVD giữ lại bao nhiêu phần thông tin của dữ liệu
- tổng cộng các thành phần đang giữ lại được bao nhiêu phần biến thiên

Điều này rất quan trọng vì giảm chiều không có nghĩa là giảm càng nhiều càng tốt, mà phải:

- giảm hợp lý
- nhưng vẫn giữ được mức thông tin đủ lớn

Nhờ explained variance, em có thể đánh giá được mức độ hiệu quả của số chiều mới sau giảm chiều.

## 19. Kết quả đạt được

Sau khi hoàn thành phần 1 và phần 2, bài làm đạt được các kết quả sau:

- dữ liệu được làm sạch và chuẩn hóa
- text được chuẩn bị tốt hơn cho vector hóa
- tạo được feature matrix từ dữ liệu review
- áp dụng được Truncated SVD để giảm chiều
- và xuất ra dữ liệu mới có số chiều thấp hơn

Như vậy, đề tài không chỉ dừng ở phần tiền xử lý dữ liệu mà đã đi trọn được chu trình:  
**cleaning → normalization → feature matrix → dimensionality reduction bằng SVD**

## 20. Ý nghĩa 

Đề tài này có ý nghĩa ở cả mặt lý thuyết lẫn thực hành.

### Về mặt lý thuyết

- giúp hiểu rõ khái niệm giảm chiều dữ liệu
- hiểu bản chất của SVD
- và thấy được vì sao SVD quan trọng trong phân tích dữ liệu lớn

### Về mặt thực hành

- áp dụng được trên dữ liệu review thực tế
- xử lý được dữ liệu văn bản có số chiều cao
- tạo ra biểu diễn dữ liệu gọn hơn cho các bước phân tích, phân loại hoặc gợi ý sau này


## 22. Kết luận

Tóm lại, trong đề tài này đã:

- thực hiện phần 1 là làm sạch và chuẩn hóa dữ liệu
- thực hiện phần 2 là giảm chiều dữ liệu bằng Truncated SVD
- và áp dụng trên Amazon Product Reviews Dataset

Qua đó,  thấy rằng:

- dữ liệu review sau khi vector hóa có số chiều rất lớn
- vì vậy rất cần kỹ thuật giảm chiều
- và SVD là một phương pháp phù hợp để giữ lại thông tin quan trọng trong khi giảm đáng kể số chiều của dữ liệu

Nói ngắn gọn, đề tài đã cho thấy:  
**SVD là công cụ hiệu quả để xử lý dữ liệu lớn, đặc biệt là dữ liệu văn bản có ma trận đặc trưng thưa và số chiều cao.**


