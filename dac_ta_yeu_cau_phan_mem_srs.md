# 03. Đặc Tả Yêu Cầu Phần Mềm (SRS - Software Requirements Specification)

## 1. Giới Thiệu
Tài liệu này đặc tả chi tiết chức năng và kỹ thuật cho dự án **Nhận diện Ngôn ngữ Ký hiệu Real-time dựa trên YOLOv8**.

---

## 2. Yêu Cầu Chức Năng (Functional Requirements)

### FR-01: Quản lý Luồng Video (Webcam Stream)
* **Mô tả:** Hệ thống cho phép mở camera, stream luồng hình ảnh tới server với độ phân giải tối thiểu 640x480.
* **Đầu vào:** Quyền camera từ trình duyệt.
* **Đầu ra:** Màn hình video trực tiếp.

### FR-02: Nhận diện Ký hiệu & Hiển thị Bounding Box
* **Mô tả:** Server chạy mô hình YOLOv8, trả về tọa độ khung hình chữ nhật xung quanh bàn tay và nhãn ký hiệu kèm % độ tin cậy.
* **Thời gian phản hồi:** ≤ 50ms per frame.

### FR-03: Ghép Ký hiệu Thành Văn bản Complete
* **Mô tả:** Hệ thống tự động gom nhóm các ký hiệu đứng yên trong 1 giây để tạo thành từ/câu hoàn chỉnh.

### FR-04: Đọc câu văn bản (Text-to-Speech)
* **Mô tả:** Cho phép bấm nút đọc câu đã dịch ra âm thanh bằng Web Speech API.

---

## 3. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)
* **FPS:** Tối thiểu 30 frames/giây.
* **Độ chính xác:** mAP@0.5 ≥ 88%.
* **Khả năng mở rộng:** Tách biệt hoàn toàn Frontend và AI Backend Service.