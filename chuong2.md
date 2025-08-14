# Chương 2: Triển khai mô phỏng phân tích tương quan & hồi quy

## 2.1 Mô tả dữ liệu và nguồn thu thập

Trong nghiên cứu này, chúng tôi thực hiện mô phỏng dữ liệu nhằm minh họa quá trình phân tích tương quan và hồi quy tuyến tính đơn.  
Hai biến số được xem xét bao gồm:

- **Biến X (Height)**: Chiều cao tính bằng cm.
- **Biến Y (Weight)**: Cân nặng tính bằng kg.

Dữ liệu được sinh ngẫu nhiên dựa trên giả định mối quan hệ tuyến tính:
$$ Y = 0.45X - 50 + \varepsilon $$

trong đó:
- $ \varepsilon \sim \mathcal{N}(0, \sigma^2) $, với $ \sigma = 4 $ kg.
- Số lượng quan sát: $ n = 30 $.

**Bảng 2.1 – Trích mẫu dữ liệu quan sát**

| STT | Height (cm) | Weight (kg) |
|-----|-------------|-------------|
| 1   | 174.82      | 30.20       |
| 2   | 168.00      | 25.34       |
| ... | ...         | ...         |
| 30  | 165.90      | 24.80       |

(Nguồn dữ liệu: Mô phỏng bằng Python, xem chi tiết ở Phụ lục A)



## 2.2 Tiền xử lý dữ liệu

Dữ liệu được kiểm tra qua các bước:

1. **Giá trị thiếu (Missing values):** Không phát hiện.
2. **Ngoại lai (Outliers):** Xác định bằng phương pháp IQR. Không có giá trị nào vượt ngưỡng $ Q_1 - 1.5 \times IQR $ hoặc $ Q_3 + 1.5 \times IQR $.
3. **Đồng nhất đơn vị đo:** Cả hai biến đều ở đơn vị gốc, không cần chuẩn hóa.



## 2.3 Phân tích tương quan

### 2.3.1 Công thức
Hệ số tương quan Pearson được tính bằng:
$$ r_{xy} = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{(n-1) s_x s_y} $$

trong đó:
- $ \bar{x} $, $ \bar{y} $: Trung bình mẫu của $ X $ và $ Y $.
- $ s_x $, $ s_y $: Độ lệch chuẩn mẫu.
- $ n $: Số quan sát.

### 2.3.2 Kết quả tính toán
- $ \bar{x} = 168.683 $, $ s_x = 6.300 $  
- $ \bar{y} = 25.423 $, $ s_y = 4.899 $  
- Hiệp phương sai: $ s_{xy} = 20.180 $  
- Hệ số tương quan Pearson: $ r = 0.6539 $

### 2.3.3 Kiểm định ý nghĩa
Giả thuyết:
- $ H_0: \rho = 0 $ (không có tương quan)
- $ H_1: \rho \neq 0 $ (có tương quan)

Thống kê kiểm định:
$$ t = \frac{r\sqrt{n-2}}{\sqrt{1-r^2}} = 4.5733, \quad df = 28 $$

p-value = $ 8.90 \times 10^{-5} $ < 0.05  
→ Bác bỏ $ H_0 $, chấp nhận $ H_1 $: có mối tương quan tuyến tính dương có ý nghĩa thống kê.


## 2.4 Xây dựng mô hình hồi quy tuyến tính đơn

### 2.4.1 Phương pháp
Phương pháp bình phương tối thiểu (OLS) tìm $ b_0, b_1 $ để tối thiểu hóa:
$$ SSE = \sum_{i=1}^n (y_i - \hat{y}_i)^2 $$

### 2.4.2 Công thức
$$ b_1 = \frac{s_{xy}}{s_x^2}, \quad b_0 = \bar{y} - b_1 \bar{x} $$

### 2.4.3 Kết quả mô hình
- $ b_1 = 0.5094 $  
- $ b_0 = -60.3382 $

Phương trình hồi quy:
$$ \hat{Y} = -60.3382 + 0.5094 X $$

**Bảng 2.2 – Kết quả ước lượng tham số**

| Tham số | Ước lượng | Sai số chuẩn | t-stat | p-value |
|---------|-----------|--------------|--------|---------|
| $ b_0 $ | -60.3382  | 14.302       | -4.218 | 0.0002  |
| $ b_1 $ | 0.5094    | 0.1114       | 4.573  | 0.00009 |


## 2.5 Độ phù hợp mô hình

Hệ số xác định:
$$ R^2 = \frac{SSR}{SST} = r^2 = 0.4276 $$

Ý nghĩa: 42.76% biến thiên của cân nặng được giải thích bởi chiều cao, phần còn lại do yếu tố khác.


## 2.6 Minh họa kết quả

**Hình 2.1 – Biểu đồ phân tán và đường hồi quy**

(Chèn hình `ch02_regression.png`)


## 2.7 Thảo luận

1. **Kết quả:** Mối tương quan dương vừa phải, phù hợp giả định ban đầu.
2. **Giới hạn:** $ R^2 $ chưa cao, mô hình chưa tính đến nhiều yếu tố quan trọng khác.
3. **Hướng mở rộng:** Dùng hồi quy đa biến, thêm các biến như độ tuổi, giới tính, chế độ ăn uống.



## 2.8 Kết luận chương

Chương này đã triển khai mô phỏng dữ liệu, phân tích tương quan và xây dựng mô hình hồi quy tuyến tính đơn.  
Kết quả chỉ ra mối quan hệ tuyến tính có ý nghĩa thống kê giữa chiều cao và cân nặng, nhưng độ giải thích còn hạn chế.  
Nghiên cứu tiếp theo sẽ mở rộng mô hình và kiểm tra các giả định hồi quy để tăng độ chính xác.