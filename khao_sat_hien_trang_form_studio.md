# 01. Khảo Sát Hiện Trạng & Yêu Cầu Đầu Vào

## 1. Bối cảnh & Khảo sát Hiện trạng
Hiện nay, người khiếm thính gặp nhiều rào cản trong giao tiếp hằng ngày do đa số cộng đồng không hiểu ngôn ngữ ký hiệu. 
* **Hạn chế của giải pháp truyền thống:** Việc thuê phiên dịch viên trực tiếp tốn chi phí lớn và không sẵn có trong mọi tình huống.
* **Hạn chế của ứng dụng cũ:** Các ứng dụng tra cứu từ điển ký hiệu dạng tĩnh chưa hỗ trợ dịch tự động hai chiều theo thời gian thực (Real-time).

---

## 2. Nhu cầu Người dùng (Form Survey Summary)

Khảo sát được thực hiện trên nhóm đối tượng: Người khiếm thính, người thân, và cộng đồng hỗ trợ người khuyết tật.

### 2.1. Kết quả chính
1. **88%** Mong muốn có ứng dụng chạy trực tiếp trên trình duyệt Web hoặc Điện thoại mà không cần cài đặt phức tạp.
2. **92%** Yêu cầu độ trễ nhận diện phải cực thấp (< 0.2 giây) để không ngắt quãng cuộc trò chuyện.
3. **75%** Cần tính năng đọc thành tiếng (Text-to-Speech) để giao tiếp trực tiếp với người không biết chữ hoặc không nhìn màn hình.

---

## 3. Ma trận Đánh giá Giải pháp Kỹ thuật

| Tiêu chí | Nhận diện bằng MediaPipe + LSTM | Nhận diện bằng YOLOv8 |
| :--- | :--- | :--- |
| **Tốc độ xử lý (FPS)** | Trung bình (15 - 25 FPS) | Cao (30 - 60+ FPS) |
| **Phát hiện bàn tay** | Phụ thuộc vào Landmark | Khoanh vùng trực tiếp (Bounding Box) |
| **Triển khai Web** | Khá phức tạp | Rất dễ tích hợp qua ONNX/FastAPI |
| **Độ chính xác** | 80 - 85% | 88 - 95% |

**Kết luận khảo sát:** Lựa chọn **YOLOv8** kết hợp **FastAPI (WebSockets)** và **React.js** làm bộ giải pháp kỹ thuật chính.