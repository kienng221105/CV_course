# Lab 02 - Nhiệm vụ cần làm và kiến thức đạt được

Tài liệu này tổng hợp hai PDF trong thư mục `lab2`, chia theo từng nhiệm vụ thực hành. Các trang được dẫn theo số trang in trong PDF.

## Tài liệu đã đọc

1. `arithmetic-logical_part1.pdf` - Lab 02 Part 1: Arithmetics & Logical Operations (9 trang).
2. `Lab02_part2_vi_Geometric_Transforms.pdf` - Lab 02 Part 2: 2D Geometric Transformations (8 trang).

## Chuẩn bị chung

- Cài `numpy`, `opencv-python` và `matplotlib` theo hướng dẫn của PDF.
- Dùng ảnh xám: mỗi ảnh là ma trận cường độ 2D, thường có kiểu `uint8` trong khoảng `[0, 255]`.
- Với phép tính tự cài bằng NumPy, chuyển ảnh sang `float32` trước khi tính; sau đó giới hạn/chuẩn hóa miền giá trị rồi chuyển về `uint8`. Tính trực tiếp trên `uint8` có thể gây tràn hoặc hụt số.
- Hiển thị kết quả cạnh ảnh gốc, đặt tiêu đề và lưu lại để tiện so sánh.
- Trong Part 1, với mỗi phương pháp được nêu, đối chiếu bản NumPy tự cài với hàm OpenCV tương ứng.

---

# Phần 1 - Phép toán số học, logic và phân ngưỡng Otsu

Nguồn: `arithmetic-logical_part1.pdf`.

## Nhiệm vụ 1 - Gamma correction

**Việc cần làm**

1. Chọn và đọc một ảnh xám `input.png`.
2. Cài phép biến đổi `s = r^gamma`, với `r` được chuẩn hóa về `[0, 1]`:
   - Bản NumPy: tính lũy thừa trên mảng float, giới hạn kết quả và đổi lại `uint8`.
   - Bản OpenCV: tạo bảng tra cứu (LUT) 256 mức rồi áp dụng bằng `cv2.LUT`.
3. Chạy với `gamma` lần lượt là `0.5`, `0.8`, `1.2`, `2.0`; so sánh với ảnh gốc.
4. Trả lời vùng nào thay đổi nhiều nhất và giải thích xu hướng sáng/tối.

**Sau nhiệm vụ này, học được**

- Gamma là biến đổi phi tuyến theo cường độ pixel.
- `gamma < 1` làm sáng vùng tối; `gamma > 1` làm tối tổng thể theo cách nén vùng tối; `gamma = 1` giữ nguyên ảnh.
- Gamma nhỏ có thể làm nhiễu ở vùng tối lộ rõ hơn. Gamma cũng có thể làm histogram thuận lợi hoặc bất lợi hơn cho phân đoạn.

**Tham chiếu:** trang 3-4.

## Nhiệm vụ 2 - Cộng, trừ, nhân, chia hai ảnh

**Việc cần làm**

1. Chọn hai ảnh xám `A`, `B` có cùng kích thước.
2. Tính bốn kết quả: `A+B`, `A-B`, `A*B`, `A/(B+epsilon)`.
3. Cài bằng NumPy theo quy trình float an toàn và bằng các hàm OpenCV tương ứng (`cv2.add`, `cv2.subtract`, `cv2.multiply`, `cv2.divide`).
4. Hiển thị/so sánh kết quả của hai cách và mô tả khác biệt do kiểu dữ liệu, hệ số, clipping hoặc phép toán bão hòa.
5. Trả lời câu hỏi trong tài liệu:
   - Vì sao phép trừ hữu ích cho phát hiện thay đổi?
   - Vì sao phép chia dễ tạo ra giá trị lớn?
   - Khi nào clipping làm mất thông tin có ích?

**Sau nhiệm vụ này, học được**

- Phép toán được thực hiện theo từng pixel giữa hai ảnh.
- Cộng có thể dùng để tăng sáng/trộn ảnh; trừ làm nổi phần khác biệt; nhân kết hợp cường độ như một mặt nạ mềm; chia có thể chuẩn hóa theo ảnh tham chiếu nhưng nhạy với mẫu số nhỏ.
- NumPy float rồi clip và phép toán số nguyên bão hòa của OpenCV không nhất thiết cho cùng kết quả. Cần xử lý mẫu số gần 0 và kiểm soát dải giá trị.

**Tham chiếu:** trang 4-6.

## Nhiệm vụ 3 - Nhị phân hóa và phép toán logic

**Việc cần làm**

1. Từ cùng một ảnh, tạo hai ảnh nhị phân bằng hai ngưỡng khác nhau. Theo định nghĩa trong tài liệu, pixel `>= t` thành `255`, còn lại thành `0`.
2. Tính AND, OR, XOR cho cặp mặt nạ:
   - Bản NumPy: biểu thức điều kiện logic.
   - Bản OpenCV: `cv2.bitwise_and`, `cv2.bitwise_or`, `cv2.bitwise_xor`.
3. So sánh kết quả và giải thích:
   - AND giữ pixel được chọn ở cả hai mặt nạ (đồng thuận/vùng chắc chắn).
   - OR gộp pixel được chọn ở ít nhất một mặt nạ (hợp các ứng viên).
   - XOR giữ chỗ hai mặt nạ bất đồng.
4. Thử dùng mặt nạ với ảnh xám để trích xuất ROI (vùng quan tâm), nếu phù hợp với dữ liệu đã chọn.

**Sau nhiệm vụ này, học được**

- AND/OR/XOR có ý nghĩa dễ diễn giải nhất khi đầu vào là mặt nạ nhị phân 0/255.
- Mặt nạ có thể kết hợp nhiều điều kiện hoặc giới hạn ảnh vào ROI.
- AND phù hợp để lấy phần giao/đồng thuận; OR để hợp vùng; XOR để tìm khác biệt giữa mặt nạ.

**Tham chiếu:** trang 6-7.

## Nhiệm vụ 4 - Tự cài đặt Otsu và so sánh với OpenCV

**Việc cần làm**

1. Tự cài Otsu bằng NumPy: lập histogram 256 mức, tính xác suất tích lũy và phương sai giữa hai lớp cho từng ngưỡng; chọn ngưỡng làm phương sai giữa lớp lớn nhất.
2. Dùng ngưỡng tìm được để tạo ảnh nhị phân.
3. Cài/áp dụng bản OpenCV với `cv2.threshold(..., THRESH_BINARY + THRESH_OTSU)`.
4. So sánh ngưỡng và mặt nạ giữa hai cách trên cùng ảnh.
5. Chạy Otsu trên ảnh gốc, sau đó gamma correction rồi Otsu; so sánh ngưỡng và kết quả. Giải thích khi nào gamma làm việc phân ngưỡng dễ hoặc khó hơn.
6. Nhận xét chất lượng Otsu trên ảnh đang dùng, dựa vào histogram và độ đồng đều ánh sáng.

**Sau nhiệm vụ này, học được**

- Otsu tự chọn một ngưỡng toàn cục bằng cách tối đa hóa phương sai giữa nền và tiền cảnh.
- Otsu thường phù hợp khi histogram có hai cụm rõ, ánh sáng tương đối đều và nhiễu vừa phải.
- Bóng/ánh sáng không đồng đều, cường độ hai lớp chồng lấn, vật thể quá nhỏ hoặc histogram không có hai cụm rõ có thể làm Otsu thất bại.
- Tiền xử lý gamma thay đổi histogram, vì vậy có thể cải thiện hoặc làm kém kết quả; phải đánh giá bằng ảnh đầu ra chứ không chỉ nhìn ngưỡng số.

**Tham chiếu:** trang 7-9.

## Nhiệm vụ 5 - Bốn pipeline tích hợp

Thực hiện bốn bài tập tích hợp ở cuối Part 1; lưu ảnh trung gian và ảnh cuối để thể hiện từng bước.

### 5.1 Làm sáng rồi phân ngưỡng

- Áp dụng gamma nhỏ hơn 1, ví dụ `0.6`.
- Chạy Otsu trên ảnh đã hiệu chỉnh.
- So sánh mask với Otsu trực tiếp trên ảnh gốc.

**Học được:** cách kết hợp biến đổi cường độ với phân đoạn; đánh giá tiền xử lý có giúp tách nền/đối tượng hay không.

### 5.2 Trích ROI bằng AND

- Tạo mask bằng Otsu.
- AND mask với ảnh gốc để chỉ giữ cường độ ảnh ở vùng tiền cảnh.

**Học được:** dùng ảnh nhị phân như mặt nạ để trích vùng quan tâm từ ảnh xám.

### 5.3 Phát hiện thay đổi

- Dùng hai ảnh đã căn chỉnh `A`, `B`.
- Tính sai khác tuyệt đối `|A-B|`.
- Chạy Otsu trên ảnh sai khác để thu mask vùng thay đổi.

**Học được:** kết hợp sai khác theo pixel với phân ngưỡng; căn chỉnh ảnh là điều kiện quan trọng để tránh báo thay đổi giả do lệch vị trí.

### 5.4 Hợp nhất nhiều mask

- Tạo `mask1` (ví dụ Otsu trên ảnh gốc) và `mask2` (ví dụ Otsu sau gamma).
- Tạo kết quả OR, AND, XOR và diễn giải ứng viên hợp nhất, vùng đồng thuận, vùng bất đồng.

**Học được:** cách hợp nhất các dự đoán phân đoạn và ý nghĩa thực tế khác nhau của từng phép logic.

**Tham chiếu:** trang 9.

---

# Phần 2 - Biến đổi hình học 2D

Nguồn: `Lab02_part2_vi_Geometric_Transforms.pdf`.

## Nhiệm vụ 6 - Translation (tịnh tiến)

**Việc cần làm**

1. Hiểu công thức `x' = x + tx`, `y' = y + ty`.
2. Cài phiên bản NumPy cơ bản bằng cách duyệt pixel từ nguồn sang đích, bỏ qua tọa độ nằm ngoài canvas.
3. Cài phiên bản inverse mapping: với mỗi pixel đích, tìm tọa độ nguồn tương ứng (`src_x = x - tx`, `src_y = y - ty`).
4. Dịch ảnh sang phải 50 pixel và lên trên 30 pixel; so sánh với `cv2.warpAffine`.
5. Dùng cùng kích thước canvas như ví dụ PDF và ghi nhận vùng bị cắt/điền giá trị biên.

**Sau nhiệm vụ này, học được**

- Tịnh tiến dịch tọa độ pixel theo vector `(tx, ty)`.
- Forward mapping có thể bỏ sót vị trí đích; inverse mapping kiểm tra từng pixel đích nên hạn chế pixel holes.
- Kích thước canvas và cách xử lý biên quyết định ảnh có bị cắt hay phần trống được điền bằng gì.

**Tham chiếu:** trang 2-3.

## Nhiệm vụ 7 - Scaling (thay đổi tỉ lệ)

**Việc cần làm**

1. Cài scaling cơ bản với `x' = sx*x`, `y' = sy*y` trên canvas kích thước cũ.
2. Cài `scale_canvas` tạo canvas mới có kích thước gần `W*sx` x `H*sy`, dùng inverse mapping để lấy pixel nguồn cho mỗi pixel đích.
3. Chạy OpenCV `cv2.resize` với `fx=1.5`, `fy=1.5`, ít nhất một chế độ nội suy.
4. So sánh `INTER_NEAREST`, `INTER_LINEAR`, `INTER_CUBIC` về độ mượt, chi tiết và thời gian nếu cần.
5. Quan sát khác biệt giữa canvas cũ (có thể crop/mất pixel) và canvas được điều chỉnh theo tỉ lệ.

**Sau nhiệm vụ này, học được**

- Scaling thay đổi tọa độ và kích thước ảnh; co ảnh và phóng ảnh có thể phát sinh lấy mẫu thiếu hoặc lặp pixel.
- Nội suy quyết định cách ước lượng cường độ tại tọa độ không nguyên; nearest giữ pixel gốc, linear/cubic thường cho kết quả mượt hơn nhưng có chi phí tính toán cao hơn.
- Kích thước canvas phải tương thích với tỉ lệ nếu muốn giữ toàn bộ nội dung.

**Tham chiếu:** trang 4-5.

## Nhiệm vụ 8 - Rotation (xoay)

**Việc cần làm**

1. Cài phép quay theo công thức lượng giác trong tài liệu; bản cơ bản quay quanh tâm ảnh và ghi pixel lên canvas cùng kích thước.
2. Chạy ví dụ quay 45 độ bằng `cv2.getRotationMatrix2D` và `cv2.warpAffine`.
3. So sánh ảnh tự cài với OpenCV, chú ý canvas cố định, crop, biên đen, làm tròn tọa độ và pixel holes.

**Sau nhiệm vụ này, học được**

- Phép quay được biểu diễn bằng ma trận affine, thường lấy tâm ảnh làm tâm quay.
- Quay trên canvas không mở rộng có thể làm mất góc ảnh; cách ánh xạ và nội suy ảnh hưởng chất lượng.
- OpenCV cung cấp phép biến đổi tối ưu và hỗ trợ nội suy khi ánh xạ tọa độ.

**Tham chiếu:** trang 5-6.

## Nhiệm vụ 9 - Skew / Shear (biến dạng xiên)

**Việc cần làm**

1. Cài công thức `x' = x + k*y`, `y' = y` bằng NumPy.
2. Cài bản OpenCV với ma trận affine `[[1, k, 0], [0, 1, 0]]`.
3. Dùng `k=0.3`; trong OpenCV, tạo canvas rộng hơn theo công thức mẫu trong tài liệu.
4. So sánh kết quả về độ nghiêng, phần ảnh bị crop và vùng biên trống.

**Sau nhiệm vụ này, học được**

- Shear làm tọa độ ngang phụ thuộc vào tọa độ dọc, khiến các hàng dịch ngang tương đối với nhau.
- Canvas ban đầu có thể không đủ chứa ảnh sau shear; cần tính lại kích thước đầu ra và xác định cách xử lý biên.

**Tham chiếu:** trang 6-7.

## Nhiệm vụ 10 - Thứ tự phép biến đổi và affine tổng quát

**Việc cần làm**

1. So sánh hai pipeline `R(30°) → S(1.5)` và `S(1.5) → R(30°)`; giải thích bằng ảnh kết quả vì sao đổi thứ tự cho kết quả khác.
2. So sánh `T → R` với `R → T`.
3. Kết hợp `T(40,20)`, `R(45°)`, `S(0.5)` thành một chuỗi biến đổi; nêu rõ thứ tự đang áp dụng.
4. Cài hàm tổng quát `affine_transform(img, matrix)` để nhận ma trận affine và áp dụng biến đổi.
5. So sánh tốc độ giữa thuật toán NumPy tự cài và `cv2.warpAffine` trên cùng ảnh/kích thước; ghi cách đo và kết quả.

**Sau nhiệm vụ này, học được**

- Biến đổi hình học không giao hoán: đổi thứ tự quay, tịnh tiến, co giãn có thể tạo kết quả khác.
- Có thể ghép nhiều phép affine bằng phép nhân ma trận, nhưng phải thống nhất quy ước tọa độ, tâm quay, thứ tự nhân và canvas.
- Vòng lặp NumPy thuần giúp hiểu thuật toán; OpenCV tối ưu cho xử lý thực tế. So sánh tốc độ cần đo cùng dữ liệu và cùng điều kiện.

**Tham chiếu:** trang 7-8.

---

# Gợi ý cách nộp/đối chiếu kết quả

PDF không quy định định dạng nộp cụ thể. Để chứng minh đã hoàn thành đầy đủ, nên chuẩn bị cho mỗi nhiệm vụ: mã nguồn, ảnh đầu vào, ảnh kết quả có tiêu đề, tham số đã dùng và một nhận xét ngắn trả lời câu hỏi của đề. Với các nhiệm vụ so sánh NumPy/OpenCV, đặt hai đầu ra cạnh nhau và ghi nhận điểm khác nhau thay vì chỉ ghi rằng chúng giống nhau.

## Lưu ý về nội dung tài liệu

- PDF Part 1 liệt kê trong mục tiêu học tập việc hiểu tích chập 2D và cài đặt chế độ `VALID`/`SAME`, nhưng các trang hướng dẫn và bài tập trong chính PDF này không có phần nhiệm vụ/tích chập tương ứng. Vì vậy tài liệu tổng hợp này không thêm bài tích chập ngoài nội dung PDF; hãy kiểm tra với giảng viên nếu đây là yêu cầu chấm điểm.
- Hai PDF ghi niên khóa khác nhau: Part 1 là `2025-2026`, Part 2 là `2026-2027`.
- Một số đoạn mã minh họa trong PDF chỉ là khung ý tưởng. Chẳng hạn, cài đặt forward mapping cần kiểm tra cả giới hạn dưới và trên của tọa độ đích; khi tự triển khai nên kiểm tra chỉ số hợp lệ để tránh ghi ngoài mảng.
