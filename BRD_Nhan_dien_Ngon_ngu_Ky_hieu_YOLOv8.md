TRƯỜNG ĐẠI HỌC CÔNG NGHỆ
THÔNG TIN VÀ TRUYỀN THÔNG
KHOA CÔNG NGHỆ THÔNG TIN
BRD
BUSINESS REQUIREMENTS DOCUMENT (BRD)
ỨNG DỤNG NHẬN DIỆN NGÔN NGỮ KÝ HIỆU DỰA TRÊN YOLOv8
Sinh viên thực hiện: Phạm Quang Duy
Giảng viên hướng dẫn: TS. Quách Xuân Trưởng
Lớp: CN CNTT K22A
Phiên bản: 1.0 | Ngày lập: 09/11/2026

## Kiểm soát tài liệu

| Thuộc tính | Nội dung |
| --- | --- |
| Tên hệ thống | ỨNG DỤNG NHẬN DIỆN NGÔN NGỮ KÝ HIỆU DỰA TRÊN YOLOv8 |
| Mã tài liệu | BRD-YOLO-SIGN-01 |
| Phiên bản | 1.0 |
| Nguồn yêu cầu | Đề cương đồ án H02.Hệ thống AI và ứng dụng; sơ đồ luồng hoạt động do người dùng cung cấp |
| Trạng thái | Bản dự thảo để rà soát và thống nhất |

## 1. Tóm tắt điều hành

Tài liệu BRD mô tả nhu cầu nghiệp vụ đối với ứng dụng hỗ trợ nhận diện các cử chỉ ngôn ngữ ký hiệu cơ bản. Ứng dụng sử dụng mô hình phát hiện đối tượng YOLOv8n, cung cấp giao diện web bằng Python/Streamlit và chuyển nhãn ký hiệu được nhận diện thành văn bản, đồng thời phát âm bằng tiếng Việt.
Theo sơ đồ luồng được cung cấp, người dùng có thể đưa dữ liệu vào từ webcam, video có sẵn hoặc hình ảnh. Kết quả được mô hình xử lý, hiển thị khung nhận diện, tên ký hiệu và độ tin cậy; sau đó có thể xem dưới dạng văn bản hoặc nghe âm thanh.

## 2. Bối cảnh và vấn đề nghiệp vụ

Người khiếm thính và người giao tiếp bằng ngôn ngữ ký hiệu có thể gặp rào cản khi trao đổi với người chưa biết ký hiệu. Đề tài hướng tới một công cụ hỗ trợ nhận biết một tập cử chỉ giới hạn, hiển thị nhãn dễ hiểu và đọc nhãn bằng tiếng Việt. Phạm vi này là công cụ hỗ trợ, không thay thế phiên dịch viên hoặc hệ thống dịch ngôn ngữ ký hiệu đầy đủ.

## 3. Mục tiêu nghiệp vụ

- Xây dựng ứng dụng máy tính/web dễ sử dụng để nhận diện từ 5 đến 8 cử chỉ ký hiệu cơ bản.
- Hỗ trợ nhận diện theo thời gian thực từ webcam và xử lý ảnh; theo sơ đồ giao diện, bổ sung xử lý video có sẵn.
- Hiển thị bounding box, tên ký hiệu tiếng Việt và độ tin cậy dự đoán.
- Chuyển kết quả nhận diện thành văn bản và phát tiếng nói tiếng Việt.
- Đạt mục tiêu kỹ thuật trong đề cương: mAP50 trên 85% và tốc độ mục tiêu 20–30 FPS trên máy tính thông thường; các chỉ tiêu phải được xác minh bằng kiểm thử thực tế.

## 4. Các bên liên quan

| Bên liên quan | Vai trò / nhu cầu |
| --- | --- |
| Người dùng cuối | Mở webcam hoặc tải ảnh/video; xem kết quả; nghe phát âm. |
| Sinh viên phát triển | Chuẩn bị dữ liệu, huấn luyện YOLOv8, xây dựng giao diện, tích hợp TTS và kiểm thử. |
| Giảng viên hướng dẫn / hội đồng | Đánh giá yêu cầu, kiến trúc, chất lượng mô hình, kiểm thử và sản phẩm đồ án. |
| Người hỗ trợ giao tiếp | Sử dụng kết quả như tín hiệu tham khảo trong trao đổi cơ bản. |

## 5. Phạm vi sản phẩm

### 5.1 Trong phạm vi

- Nhận diện 5–8 lớp cử chỉ tay đã được định nghĩa và gán nhãn trong dataset.
- Nguồn vào: webcam, tệp hình ảnh JPG/PNG và tệp video phổ biến (phạm vi video cần xác nhận khi triển khai).
- Phát hiện vị trí cử chỉ bằng bounding box và phân loại nhãn.
- Hiển thị tên ký hiệu và độ tin cậy theo phần trăm.
- Hiển thị kết quả ở dạng văn bản; phát âm tiếng Việt bằng thư viện TTS như gTTS hoặc pyttsx3.
- Cho phép người dùng bắt đầu/dừng webcam, chọn tệp đầu vào và xem kết quả xử lý.
- Đóng gói mã nguồn, trọng số mô hình best.pt, báo cáo và slide bảo vệ.

### 5.2 Ngoài phạm vi

- Dịch đầy đủ ngôn ngữ ký hiệu liên tục thành câu tiếng Việt có ngữ pháp tự nhiên.
- Nhận diện mọi ký hiệu trong mọi ngôn ngữ ký hiệu hoặc mọi biến thể vùng miền.
- Định danh người dùng, lưu hồ sơ cá nhân hoặc thu thập dữ liệu sinh trắc học.
- Tích hợp API thương mại hoặc triển khai dịch vụ đám mây quy mô lớn, trừ khi có yêu cầu bổ sung.

## 6. Quy trình nghiệp vụ mục tiêu

1. Người dùng mở ứng dụng.
1. Người dùng chọn một trong các nguồn: webcam, video hoặc hình ảnh.
1. Ứng dụng lấy khung hình/ảnh và chuẩn hóa đầu vào cho mô hình.
1. YOLOv8n dự đoán vị trí đối tượng, lớp ký hiệu và độ tin cậy.
1. Ứng dụng vẽ bounding box và hiển thị nhãn cùng độ tin cậy.
1. Ứng dụng đưa nhãn nhận diện được vào vùng văn bản.
1. Khi được kích hoạt theo thiết kế, TTS phát âm nhãn bằng tiếng Việt.
1. Người dùng có thể tiếp tục nhận diện, dừng xử lý hoặc chọn nguồn đầu vào khác.

## 7. Yêu cầu nghiệp vụ và tiêu chí thành công

| Mã | Yêu cầu / tiêu chí | Cách xác minh |
| --- | --- | --- |
| BR-01 | Hỗ trợ nhận diện 5–8 cử chỉ cơ bản. | Đối chiếu danh sách lớp trong data.yaml và kiểm thử từng lớp. |
| BR-02 | Hỗ trợ webcam và hình ảnh; luồng thiết kế có thêm video. | Kiểm thử từng nguồn vào. |
| BR-03 | Hiển thị khung, tên ký hiệu và độ tin cậy. | Quan sát kết quả trên bộ ảnh/video kiểm thử. |
| BR-04 | Có đầu ra văn bản và âm thanh tiếng Việt. | Kiểm tra văn bản và phát âm. |
| BR-05 | Mục tiêu mAP50 > 85%. | Lưu báo cáo validation/test từ YOLOv8; không dùng số liệu giả định. |
| BR-06 | Mục tiêu tốc độ 20–30 FPS trên máy tính thông thường. | Đo FPS trên cấu hình máy được ghi rõ. |
| BR-07 | Ứng dụng ổn định, dễ thao tác. | Kiểm thử luồng đầu-cuối và ghi nhận lỗi. |

## 8. Giả định, ràng buộc và rủi ro

- Dataset được gán nhãn đúng theo định dạng YOLO và có phân chia train/validation/test rõ ràng.
- Đề cương gợi ý 300–500 ảnh mỗi cử chỉ; chất lượng và độ đa dạng ảnh có ảnh hưởng trực tiếp đến khả năng tổng quát hóa.
- Mục tiêu FPS phụ thuộc phần cứng, kích thước ảnh, nguồn video và cách triển khai webcam.
- TTS trực tuyến như gTTS có thể cần Internet; TTS ngoại tuyến như pyttsx3 phụ thuộc hệ điều hành và giọng đọc cài đặt.
- Nếu dùng webcam trong trình duyệt, cần xác minh cách Streamlit truy cập webcam và quyền truy cập của trình duyệt.
- Nhận diện nhầm có thể gây hiểu sai; giao diện cần trình bày độ tin cậy và cho phép người dùng kiểm tra kết quả.

## 9. Các điểm cần xác nhận

- Chốt chính xác danh sách 5–8 lớp ký hiệu và tên tiếng Việt hiển thị.
- Chốt định dạng video đầu vào và hành vi xử lý: từng khung hình hay toàn bộ video.
- Chốt phát âm tự động ngay khi phát hiện hay chỉ phát âm khi người dùng nhấn nút.
- Chốt ngưỡng confidence và cách xử lý khi không phát hiện ký hiệu.
- Chốt môi trường chạy mục tiêu: máy tính cục bộ, Streamlit local hay máy chủ triển khai.
