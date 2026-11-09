TRƯỜNG ĐẠI HỌC CÔNG NGHỆ
THÔNG TIN VÀ TRUYỀN THÔNG
KHOA CÔNG NGHỆ THÔNG TIN
SAD
SOFTWARE ARCHITECTURE / ANALYSIS & DESIGN
ỨNG DỤNG NHẬN DIỆN NGÔN NGỮ KÝ HIỆU DỰA TRÊN YOLOv8
Sinh viên thực hiện: Phạm Quang Duy
Giảng viên hướng dẫn: TS. Quách Xuân Trưởng
Lớp: CN CNTT K22A
Phiên bản: 1.0 | Ngày lập: 09/11/2026

## Kiểm soát tài liệu

| Thuộc tính | Nội dung |
| --- | --- |
| Tên hệ thống | ỨNG DỤNG NHẬN DIỆN NGÔN NGỮ KÝ HIỆU DỰA TRÊN YOLOv8 |
| Mã tài liệu | SAD-YOLO-SIGN-01 |
| Phiên bản | 1.0 |
| Cơ sở thiết kế | Đề cương đồ án H02 và sơ đồ luồng hoạt động webcam/video/hình ảnh → YOLOv8n → kết quả → văn bản/âm thanh |
| Trạng thái | Thiết kế đề xuất; cần đối chiếu với mã nguồn triển khai cuối cùng |

## 1. Mục đích và phạm vi

SAD mô tả kiến trúc logic, các thành phần, luồng dữ liệu và quyết định thiết kế cho ứng dụng nhận diện cử chỉ ngôn ngữ ký hiệu bằng YOLOv8n. Đây là thiết kế mục tiêu cho sản phẩm đồ án; không khẳng định mọi thành phần đã được cài đặt hoàn chỉnh.

## 2. Nguyên tắc kiến trúc

- Tách giao diện người dùng, xử lý đầu vào, suy luận mô hình, trình bày kết quả và tổng hợp tiếng nói.
- Dùng YOLOv8n làm mô hình phát hiện đối tượng nhẹ, phù hợp mục tiêu xử lý gần thời gian thực.
- Dùng Streamlit để xây dựng giao diện web Python.
- Giữ trọng số best.pt và cấu hình lớp trong các tệp riêng, dễ thay thế khi huấn luyện lại.
- Không lưu hình ảnh/video của người dùng lâu dài nếu không có yêu cầu rõ ràng.

## 3. Kiến trúc tổng thể

| Tầng / thành phần | Trách nhiệm | Đầu vào | Đầu ra |
| --- | --- | --- | --- |
| UI / Streamlit | Chọn webcam, video, ảnh; điều khiển chạy/dừng; hiển thị kết quả. | Thao tác người dùng, tệp hoặc khung hình. | Yêu cầu nhận diện và vùng kết quả. |
| Input Adapter | Đọc ảnh, giải mã video hoặc nhận khung hình webcam; chuẩn hóa định dạng. | JPG/PNG, khung hình, video. | Ảnh/khung hình RGB phù hợp suy luận. |
| Inference Service | Nạp YOLOv8n và best.pt; chạy dự đoán. | Ảnh/khung hình. | Bounding box, class ID, confidence. |
| Post-processing | Ánh xạ class ID sang nhãn tiếng Việt; lọc theo confidence; chuẩn bị overlay. | Kết quả mô hình và cấu hình lớp. | Kết quả đã lọc, nhãn hiển thị. |
| Visualization | Vẽ bounding box; hiển thị ảnh, tên ký hiệu và độ tin cậy. | Ảnh gốc và kết quả sau xử lý. | Ảnh có chú thích và thông tin nhận diện. |
| Text Output | Đưa nhãn được nhận diện vào vùng văn bản. | Nhãn tiếng Việt. | Văn bản kết quả. |
| TTS Service | Chuyển nhãn thành tiếng nói tiếng Việt. | Chuỗi văn bản. | Âm thanh phát ra hoặc tệp âm thanh tạm. |
| Model/Data Assets | Lưu best.pt, cấu hình data.yaml và nhãn lớp. | Tệp mô hình và cấu hình. | Trọng số và metadata phục vụ suy luận. |

## 4. Sơ đồ thành phần

[Webcam]   [Video]   [Hình ảnh]
    \          |          /
     +---- [Input Adapter] ----+
                 |
          [Chuẩn hóa ảnh]
                 |
           [YOLOv8n / best.pt]
                 |
        [Post-processing / lọc]
                 |
        [Bounding Box + Label]
                 |
       [Tên ký hiệu + Confidence]
              /       \
      [Văn bản]       [TTS tiếng Việt]
Sơ đồ trên là biểu diễn logic. Khi triển khai, luồng webcam/video có thể cần vòng lặp khung hình; luồng ảnh tĩnh chỉ cần xử lý một lần.

## 5. Luồng xử lý chi tiết

### 5.1 Ảnh tĩnh

1. Người dùng tải tệp JPG/PNG.
1. Ứng dụng kiểm tra loại tệp và đọc ảnh.
1. Ảnh được đưa vào YOLOv8n; kích thước suy luận mặc định theo cấu hình, mục tiêu đề cương là 640×640.
1. Mô hình trả về tọa độ hộp, lớp và confidence.
1. Ứng dụng lọc kết quả theo ngưỡng cấu hình, ánh xạ lớp sang tên tiếng Việt và vẽ hộp.
1. Giao diện hiển thị ảnh đã nhận diện, văn bản và nút phát âm.

### 5.2 Webcam và video

1. Mở nguồn webcam hoặc giải mã video thành từng khung hình.
1. Với mỗi khung hình, gọi mô hình dự đoán và hậu xử lý kết quả.
1. Hiển thị khung hình đã vẽ hộp và nhãn; tính/hiển thị FPS nếu được bật.
1. Cho phép dừng luồng, giải phóng camera và kết thúc giải mã video.
1. Tránh phát âm lặp liên tục trên mọi khung hình: nên có cơ chế chống lặp, khoảng nghỉ hoặc phát âm theo thao tác người dùng.

## 6. Công nghệ và thư viện đề xuất

| Công nghệ | Vai trò / ghi chú |
| --- | --- |
| Python | Ngôn ngữ chính. |
| Streamlit | Giao diện web. |
| Ultralytics YOLOv8 | Nạp mô hình và chạy object detection. |
| OpenCV | Đọc webcam/video, chuyển đổi màu và vẽ khung khi cần. |
| Pillow (PIL) | Đọc và hiển thị ảnh tĩnh. |
| Pandas / NumPy | Xử lý dữ liệu phụ trợ nếu cần. |
| gTTS hoặc pyttsx3 | Text-to-Speech tiếng Việt; chọn một phương án sau khi kiểm tra môi trường. |
| best.pt + data.yaml | Trọng số đã huấn luyện và định nghĩa lớp. |

## 7. Cấu trúc mã nguồn đề xuất

Cấu trúc sau là đề xuất tổ chức mã nguồn, không phải khẳng định cấu trúc repository hiện tại:
sign_language_app/
├── app.py                    # Điểm vào Streamlit
├── services/
│   ├── detector.py           # Nạp YOLOv8 và suy luận
│   ├── media_input.py        # Đọc ảnh/video/webcam
│   ├── visualization.py      # Vẽ bounding box và nhãn
│   └── speech.py             # TTS tiếng Việt
├── models/
│   └── best.pt               # Trọng số mô hình đã huấn luyện
├── config/
│   └── labels.yaml           # ID lớp, tên ký hiệu, confidence mặc định
├── assets/
│   └── examples/             # Ảnh minh họa mẫu (nếu có quyền sử dụng)
├── tests/                    # Kiểm thử
├── requirements.txt
└── README.md

## 8. Thiết kế dữ liệu và cấu hình

- Mỗi kết quả phát hiện nên gồm: class_id, class_name, confidence, x1, y1, x2, y2.
- Tên lớp phải đồng nhất giữa data.yaml, mô hình best.pt và phần hiển thị tiếng Việt.
- Ngưỡng confidence nên là cấu hình có thể điều chỉnh; giá trị mặc định cần được xác định bằng kiểm thử.
- Ảnh/video đầu vào chỉ được giữ trong bộ nhớ trong quá trình xử lý, trừ khi người dùng chủ động yêu cầu lưu.
- Âm thanh tạo ra cần được giải phóng/xóa tệp tạm sau khi phát nếu thư viện TTS sinh tệp.

## 9. Xử lý lỗi và ngoại lệ

| Tình huống | Xử lý thiết kế |
| --- | --- |
| Không tìm thấy best.pt | Hiển thị thông báo cấu hình mô hình thiếu; không chạy suy luận. |
| Tệp không hỗ trợ hoặc lỗi đọc | Báo lỗi dễ hiểu và yêu cầu chọn lại tệp. |
| Không phát hiện ký hiệu | Hiển thị trạng thái “Chưa phát hiện ký hiệu phù hợp”; không đọc tiếng nói rỗng. |
| Confidence thấp | Có thể ẩn kết quả hoặc hiển thị cảnh báo theo ngưỡng đã cấu hình. |
| Webcam không truy cập được | Thông báo kiểm tra quyền camera/thiết bị; cho phép chuyển sang ảnh/video. |
| TTS lỗi hoặc không có Internet | Giữ kết quả văn bản; thông báo âm thanh không khả dụng. |
| Video kết thúc | Dừng vòng lặp và giải phóng tài nguyên. |

## 10. Hiệu năng, bảo mật và khả năng bảo trì

- Mục tiêu hiệu năng theo đề cương: 20–30 FPS; cần đo trên cấu hình máy cụ thể và phân biệt FPS suy luận với FPS giao diện.
- Mục tiêu chất lượng mô hình: mAP50 > 85%; đánh giá trên tập test độc lập.
- Dùng YOLOv8n trước do kích thước nhỏ; cân nhắc mô hình khác chỉ khi có kết quả so sánh.
- Không ghi log hình ảnh người dùng hoặc dữ liệu nhạy cảm nếu không cần thiết.
- Tách module giúp thay mô hình hoặc TTS mà không phải viết lại giao diện.

## 11. Quyết định và vấn đề mở

- Chốt dùng gTTS (cần kết nối mạng) hay pyttsx3 (phụ thuộc giọng đọc và hệ điều hành).
- Xác nhận webcam chạy qua thành phần Streamlit nào và quyền truy cập trình duyệt.
- Xác nhận video có hỗ trợ MP4/AVI hay chỉ một số định dạng.
- Chốt cơ chế chống đọc lặp và cách người dùng chủ động phát lại âm thanh.
- Xác nhận tên lớp chính thức và hình minh họa mẫu tương ứng.
