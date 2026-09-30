Ảnh đầu vào `spine.jpg` có kích thước 488 × 373 pixel, sử dụng ảnh xám 8-bit với giá trị từ 0 đến 255. Điều này cho thấy ảnh được đọc đầy đủ và có toàn bộ khoảng mức xám hợp lệ.

Histogram có 256 mức sáng và tổng số đếm là 182024, đúng bằng số pixel của ảnh. Histogram tính bằng NumPy và OpenCV cũng cho cùng tổng số pixel. Vì vậy hai cách tính cho kết quả nhất quán. Phân bố histogram nghiêng về vùng cường độ thấp, nên ảnh ban đầu có xu hướng tối.

Trên hình ảnh gốc, các chi tiết ở vùng tối khó quan sát hơn. Sau Histogram Equalization, độ tương phản được mở rộng và các chi tiết tối rõ hơn. Tuy nhiên một số vùng nhỏ và nhiễu cũng trở nên dễ nhận thấy hơn. Kết quả NumPy, OpenCV và scikit-image có cùng mục đích và cho hình ảnh sau xử lý tương tự nhau.

Với threshold thủ công, ảnh được chia thành vùng đen và trắng theo ngưỡng đã chọn. Những vùng có mức sáng gần ngưỡng có thể bị thay đổi rõ rệt nếu ngưỡng thay đổi. Otsu tự chọn ngưỡng 104; kết quả phù hợp hơn với phân bố sáng của ảnh và tạo ảnh nhị phân ổn định hơn so với việc chọn ngưỡng tùy ý.

Biến đổi tuyến tính làm thay đổi đồng thời độ sáng và độ tương phản. Kết quả cho thấy ảnh sáng hơn và tương phản mạnh hơn khi dùng hệ số tăng cường. Gamma correction cho ảnh hưởng rõ ở vùng tối: gamma nhỏ làm vùng tối sáng lên, còn gamma lớn làm ảnh tối hơn và giữ lại sự khác biệt ở các vùng sáng.

Ở Exercise 6, phép Negative đảo hoàn toàn sáng thành tối. Kết quả NumPy và OpenCV giống nhau, thể hiện qua output `True`. Log transform làm các chi tiết tối nổi bật hơn nhưng nén sự khác biệt giữa các vùng sáng. Contrast stretching dùng các mốc `P2=1.00` và `P98=255.00`; ảnh sau xử lý có tương phản rộng hơn nhưng thay đổi không quá mạnh vì ảnh vốn đã gần phủ toàn bộ khoảng 0–255.

Multi-level threshold tạo ba vùng mức xám 0, 127 và 255. Cách này giữ được nhiều thông tin hơn threshold nhị phân nhưng vẫn đơn giản hóa ảnh. Adaptive threshold cho kết quả tách vùng tốt hơn ở những khu vực có độ sáng không đồng đều; phiên bản Gaussian thường tạo biên mềm và tự nhiên hơn phiên bản local mean.

Trong Additional Homework, histogram tự cài đặt có tổng 182024 pixel và khớp hoàn toàn với OpenCV (`True`), xác nhận kết quả đếm là chính xác. Gray level modification làm sáng rõ các vùng tối, nên phù hợp để quan sát cấu trúc bị chìm trong ảnh gốc.

Median threshold cho giá trị 1. Vì phần lớn pixel thuộc vùng rất tối, ảnh nhị phân sau đó gần như bị chuyển thành trắng ở hầu hết vị trí. Kết quả này cho thấy median không phải lúc nào cũng là ngưỡng phân tách phù hợp cho ảnh có histogram lệch mạnh.

Với Power Transform, gamma 0.5 làm sáng rõ vùng tối của cả `spine.jpg` và `runway.jpg`; gamma 1.0 giữ nguyên ảnh; gamma 2.0 làm ảnh tối hơn. Trong các kết quả quan sát được, gamma 0.5 hữu ích hơn khi mục tiêu là làm rõ chi tiết tối, còn gamma 2.0 phù hợp khi cần giảm độ sáng tổng thể.

Nhìn chung, Histogram Equalization, contrast stretching và gamma nhỏ cho hiệu quả rõ nhất đối với ảnh tối. Otsu và adaptive threshold cho kết quả tách vùng đáng tin cậy hơn threshold thủ công trong trường hợp histogram hoặc độ sáng của ảnh không đồng đều.
