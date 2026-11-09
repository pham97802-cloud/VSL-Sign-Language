TRƯỜNG ĐẠI HỌC CÔNG NGHỆ
THÔNG TIN VÀ TRUYỀN THÔNG
KHOA CÔNG NGHỆ THÔNG TIN
SRS
SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
ỨNG DỤNG NHẬN DIỆN NGÔN NGỮ KÝ HIỆU DỰA TRÊN YOLOv8
Sinh viên thực hiện: Phạm Quang Duy
Giảng viên hướng dẫn: TS. Quách Xuân Trưởng
Lớp: CN CNTT K22A
Phiên bản: 1.0 | Ngày lập: 09/11/2026

## Kiểm soát tài liệu

| Thuộc tính | Nội dung |
| --- | --- |
| Tên hệ thống | ỨNG DỤNG NHẬN DIỆN NGÔN NGỮ KÝ HIỆU DỰA TRÊN YOLOv8 |
| Mã tài liệu | SRS-YOLO-SIGN-01 |
| Phiên bản | 1.0 |
| Nguồn yêu cầu | Đề cương đồ án H02; sơ đồ luồng do người dùng cung cấp |
| Trạng thái | Bản dự thảo yêu cầu để xác nhận trước khi nghiệm thu |

## 1. Giới thiệu

### 1.1 Mục đích

SRS xác định các yêu cầu chức năng, phi chức năng, giao diện, dữ liệu và tiêu chí kiểm thử cho ứng dụng nhận diện ngôn ngữ ký hiệu bằng YOLOv8n.

### 1.2 Phạm vi sản phẩm

Ứng dụng web Python/Streamlit nhận diện từ 5 đến 8 cử chỉ tay cơ bản, sử dụng webcam, ảnh tĩnh và theo sơ đồ giao diện là video có sẵn; hiển thị bounding box, tên ký hiệu, độ tin cậy; xuất kết quả văn bản và phát tiếng nói tiếng Việt.

### 1.3 Thuật ngữ

| Thuật ngữ | Định nghĩa |
| --- | --- |
| YOLOv8n | Biến thể YOLOv8 Nano dùng cho phát hiện đối tượng. |
| Bounding Box | Hộp chữ nhật định vị vùng chứa cử chỉ được phát hiện. |
| Confidence | Điểm tin cậy dự đoán của mô hình, hiển thị theo phần trăm. |
| mAP50 | Mean Average Precision tại ngưỡng IoU = 0.5. |
| FPS | Số khung hình được xử lý trong một giây. |
| TTS | Text-to-Speech, chuyển văn bản thành tiếng nói. |
| Ràng buộc lớp | Tập 5–8 nhãn ký hiệu được huấn luyện và cấu hình trong mô hình. |

## 2. Mô tả tổng quan

### 2.1 Nhóm người dùng

- Người dùng phổ thông cần nhận diện một số ký hiệu cơ bản.
- Người phát triển/quản trị kỹ thuật cài đặt mô hình và cấu hình lớp.
- Giảng viên/hội đồng kiểm thử và đánh giá sản phẩm.

### 2.2 Môi trường vận hành

- Máy tính cá nhân chạy hệ điều hành hỗ trợ Python.
- Trình duyệt web hiện đại khi dùng Streamlit.
- Webcam hoạt động nếu sử dụng nguồn webcam.
- Có Python và các thư viện được khai báo trong requirements.txt.
- Tệp trọng số best.pt phải tồn tại và tương thích với phiên bản Ultralytics cài đặt.

### 2.3 Giả định và phụ thuộc

- Dataset có nhãn đúng, các lớp không bị sai lệch giữa lúc huấn luyện và triển khai.
- Ảnh/khung hình đủ rõ để quan sát cử chỉ; các điều kiện ánh sáng và nền có thể ảnh hưởng kết quả.
- Phương án TTS được lựa chọn và kiểm tra trên máy triển khai.
- Video là yêu cầu theo sơ đồ người dùng cung cấp; cần chốt định dạng tệp hỗ trợ trước khi nghiệm thu.

## 3. Yêu cầu chức năng

| Mã | Tên yêu cầu | Mô tả yêu cầu | Ưu tiên |
| --- | --- | --- | --- |
| FR-01 | Chọn nguồn đầu vào | Hệ thống SHALL cung cấp lựa chọn webcam, tải hình ảnh và tải video theo phạm vi đã chốt. | Cao |
| FR-02 | Mở webcam | Hệ thống SHALL cho phép bắt đầu nhận diện từ webcam và dừng luồng khi người dùng yêu cầu. | Cao |
| FR-03 | Tải hình ảnh | Hệ thống SHALL cho phép tải ảnh JPG/PNG và hiển thị ảnh được chọn trước khi/hoặc sau khi xử lý. | Cao |
| FR-04 | Tải video | Hệ thống SHALL đọc video được hỗ trợ theo từng khung hình và cho phép kết thúc xử lý. | Trung bình — xác nhận |
| FR-05 | Tiền xử lý | Hệ thống SHALL chuyển ảnh/khung hình về định dạng mà mô hình yêu cầu; cấu hình mục tiêu theo đề cương là 640×640. | Cao |
| FR-06 | Nhận diện YOLOv8n | Hệ thống SHALL nạp trọng số best.pt và thực hiện suy luận trên ảnh/khung hình. | Cao |
| FR-07 | Định vị đối tượng | Hệ thống SHALL vẽ bounding box cho mỗi phát hiện hợp lệ. | Cao |
| FR-08 | Hiển thị nhãn | Hệ thống SHALL hiển thị tên ký hiệu tiếng Việt ứng với class ID được mô hình trả về. | Cao |
| FR-09 | Hiển thị độ tin cậy | Hệ thống SHALL hiển thị confidence theo phần trăm cho mỗi phát hiện được trình bày. | Cao |
| FR-10 | Đầu ra văn bản | Hệ thống SHALL hiển thị nhãn ký hiệu được nhận diện ở vùng văn bản. | Cao |
| FR-11 | Phát âm tiếng Việt | Hệ thống SHALL chuyển nhãn ký hiệu thành tiếng nói tiếng Việt bằng gTTS hoặc pyttsx3 theo phương án được chọn. | Cao |
| FR-12 | Điều khiển phát âm | Hệ thống SHOULD cung cấp cách phát âm/phát lại và tránh phát âm lặp liên tục ở mọi khung hình. | Trung bình |
| FR-13 | Không có phát hiện | Hệ thống SHALL hiển thị trạng thái rõ ràng khi không phát hiện đối tượng đạt ngưỡng. | Cao |
| FR-14 | Thông báo lỗi | Hệ thống SHALL báo lỗi khi không đọc được tệp, thiếu mô hình hoặc không mở được webcam. | Cao |
| FR-15 | Giải phóng tài nguyên | Hệ thống SHALL dừng vòng lặp và giải phóng camera/decoder khi người dùng kết thúc. | Cao |
| FR-16 | Cấu hình lớp | Hệ thống SHALL ánh xạ chính xác class ID sang tên lớp tiếng Việt của mô hình đã huấn luyện. | Cao |

## 4. Yêu cầu phi chức năng

| Mã | Thuộc tính | Yêu cầu | Mức |
| --- | --- | --- | --- |
| NFR-01 | Độ chính xác mô hình | Mục tiêu mAP50 > 85% trên tập đánh giá độc lập; báo cáo kết quả thực nghiệm thực tế. | Bắt buộc theo đề cương |
| NFR-02 | Hiệu năng | Mục tiêu 20–30 FPS trên máy tính thông thường; phải ghi cấu hình máy, kích thước ảnh và cách đo. | Bắt buộc theo đề cương |
| NFR-03 | Độ trễ giao diện | Kết quả cần hiển thị đủ nhanh để sử dụng; ngưỡng độ trễ cụ thể sẽ được xác nhận qua kiểm thử. | Cần chốt |
| NFR-04 | Khả dụng | Các thao tác chọn nguồn, bắt đầu/dừng và xem kết quả phải dễ hiểu, có nhãn rõ ràng. | Bắt buộc |
| NFR-05 | Tính ổn định | Lỗi đầu vào hoặc lỗi TTS không được làm mất toàn bộ ứng dụng; cần thông báo và cho phép tiếp tục khi phù hợp. | Bắt buộc |
| NFR-06 | Khả năng tương thích | Môi trường Python và thư viện phải được khóa/ghi rõ trong requirements.txt hoặc tài liệu cài đặt. | Bắt buộc |
| NFR-07 | Bảo mật và riêng tư | Không lưu ảnh/video đầu vào lâu dài nếu không có yêu cầu; không tải dữ liệu lên dịch vụ ngoài nếu chưa thông báo. | Bắt buộc |
| NFR-08 | Khả năng bảo trì | Mã nguồn tách biệt giao diện, suy luận, hiển thị và TTS; cấu hình lớp không hard-code rải rác. | Khuyến nghị |
| NFR-09 | Khả năng đánh giá | Cần lưu kết quả validation/test, Precision, Recall, F1 và mAP; sử dụng confusion matrix khi phù hợp. | Bắt buộc theo đề cương |

## 5. Yêu cầu giao diện

- Trang chính có khu vực chọn nguồn: Webcam, Video, Hình ảnh.
- Khu vực kết quả hiển thị ảnh/khung hình đã vẽ bounding box.
- Mỗi phát hiện có tên ký hiệu tiếng Việt và confidence dạng phần trăm.
- Có khu vực văn bản chứa kết quả nhận diện.
- Có điều khiển phát âm tiếng Việt và trạng thái âm thanh.
- Có nút bắt đầu/dừng hoặc xử lý lại phù hợp với nguồn đầu vào.
- Có thông báo dễ hiểu cho các trạng thái: đang xử lý, không phát hiện, tệp không hợp lệ, thiếu mô hình, không truy cập được camera.

## 6. Yêu cầu dữ liệu và mô hình

- Số lớp: từ 5 đến 8 cử chỉ cơ bản; tên lớp chính thức phải được thống nhất.
- Đề cương gợi ý 300–500 ảnh/cử chỉ; cần đảm bảo ảnh đa dạng về góc, khoảng cách, ánh sáng và nền.
- Đầu vào mô hình: ma trận pixel RGB; mục tiêu kích thước suy luận 640×640 theo đề cương.
- Đầu ra mô hình: bounding box, class ID/tên lớp và confidence.
- Tệp trọng số triển khai: best.pt; tệp dữ liệu/cấu hình cần chỉ rõ nhãn lớp tương ứng.
- Chia train/validation/test cần tách biệt để tránh đánh giá lạc quan do ảnh trùng hoặc gần trùng.

## 7. Quy tắc nghiệp vụ

| Mã | Quy tắc |
| --- | --- |
| BRL-01 | Chỉ hiển thị/phát âm các nhãn thuộc danh sách lớp đã được cấu hình. |
| BRL-02 | Confidence hiển thị theo thang 0–100%; cần phân biệt confidence của mô hình với xác suất đã hiệu chuẩn. |
| BRL-03 | Nếu không có phát hiện vượt ngưỡng, hiển thị thông báo không phát hiện thay vì phát âm nhãn cũ. |
| BRL-04 | Đối với webcam/video, tránh đọc lặp liên tục cùng một nhãn; cơ chế cụ thể cần được xác nhận. |
| BRL-05 | Việc phát âm không được chặn giao diện lâu đến mức làm luồng nhận diện không sử dụng được. |

## 8. Tiêu chí chấp nhận và kiểm thử

| Mã kiểm thử | Kịch bản | Kết quả mong đợi |
| --- | --- | --- |
| TC-01 | Tải ảnh JPG hợp lệ | Ảnh được đọc và trả kết quả hoặc trạng thái không phát hiện; không phát sinh lỗi chưa xử lý. |
| TC-02 | Tải ảnh PNG hợp lệ | Kết quả tương tự ảnh JPG. |
| TC-03 | Tệp ảnh không hợp lệ | Ứng dụng báo lỗi dễ hiểu và vẫn hoạt động. |
| TC-04 | Webcam hoạt động | Có khung hình, nhận diện và hiển thị kết quả; dừng được luồng. |
| TC-05 | Webcam bị từ chối/không có | Có thông báo và không treo ứng dụng. |
| TC-06 | Video được hỗ trợ | Khung hình được xử lý; có thể dừng và giải phóng tài nguyên. |
| TC-07 | Bounding box và nhãn | Hộp, tên lớp và confidence được vẽ/hiển thị theo dự đoán. |
| TC-08 | Đầu ra văn bản | Nhãn hiển thị khớp với class ID. |
| TC-09 | TTS tiếng Việt | Âm thanh phát đúng nội dung nhãn đã nhận diện. |
| TC-10 | Không phát hiện đối tượng | Thông báo không phát hiện; không đọc nhãn cũ. |
| TC-11 | Thiếu best.pt | Ứng dụng báo thiếu mô hình rõ ràng. |
| TC-12 | Đánh giá mAP50 | Báo cáo validation/test chứng minh hoặc ghi nhận không đạt mục tiêu >85%. |
| TC-13 | Đo FPS | Đo và ghi kết quả trên máy được mô tả; đối chiếu mục tiêu 20–30 FPS. |
| TC-14 | Kiểm thử dài hạn | Chạy liên tục trong khoảng thời gian thống nhất; không rò rỉ tài nguyên nghiêm trọng. |

## 9. Truy vết yêu cầu

| Nguồn / yêu cầu gốc | Yêu cầu SRS liên quan |
| --- | --- |
| Đề cương: webcam hoặc ảnh | FR-02, FR-03 |
| Sơ đồ người dùng: webcam, video, hình ảnh | FR-01 đến FR-04 |
| Bounding box, tên ký hiệu, confidence | FR-07, FR-08, FR-09 |
| Chuyển ký hiệu thành văn bản | FR-10 |
| Phát âm tiếng Việt bằng TTS | FR-11, FR-12 |
| YOLOv8n và best.pt | FR-06; mục 6 |
| mAP50 > 85% | NFR-01; TC-12 |
| 20–30 FPS | NFR-02; TC-13 |
| Kiểm thử và vận hành ổn định | NFR-05; TC-14 |

## 10. Các vấn đề cần chốt trước nghiệm thu

- Danh sách chính xác các lớp ký hiệu và cách viết nhãn tiếng Việt.
- Các định dạng video hỗ trợ và cách điều khiển phát lại/tiến trình.
- Ngưỡng confidence mặc định.
- TTS tự động hay nhấn nút; cách xử lý khi cùng một ký hiệu xuất hiện nhiều khung hình.
- Cấu hình máy chuẩn để đo FPS.
- Nguồn dữ liệu và cách chứng minh tính độc lập của tập test.
