# 02. Tài Liệu Yêu Cầu Nghiệp Vụ (BRD - Business Requirements Document)

## 1. Mục Tiêu Dự Án (Business Objectives)
* **Xây dựng hệ thống hỗ trợ giao tiếp:** Cung cấp công cụ dịch ngôn ngữ ký hiệu tự động sang văn bản/âm thanh real-time.
* **Tối ưu tốc độ & Độ chính xác:** Đạt tốc độ nhận diện ≥ 30 FPS với độ chính xác mAP@0.5 ≥ 88%.
* **Tiếp cận rộng rãi:** Phát triển trên nền tảng Web App thân thiện, hỗ trợ thiết bị di động và máy tính có webcam.

---

## 2. Phạm Vi Dự Án (Scope of Work)

### In-Scope (Trong phạm vi)
* Thu thập, chuẩn hóa và dán nhãn tập dữ liệu ngôn ngữ ký hiệu.
* Huấn luyện mô hình YOLOv8 trên tập dữ liệu ký hiệu.
* Xây dựng giao diện Web client tiếp nhận luồng video webcam và hiển thị kết quả.
* Phát triển Backend API (FastAPI) xử lý mô hình AI và WebSocket real-time.
* Tích hợp tính năng Chuyển văn bản thành giọng nói (Text-to-Speech).

### Out-of-Scope (Ngoài phạm vi)
* Dịch các câu phức tạp phụ thuộc ngữ cảnh dài (cần mô hình LLM/Transformer bổ trợ ở giai đoạn sau).
* Nhận diện đồng thời nhiều người trong môi trường thiếu sáng nặng.

---

## 3. Quy Trình Nghiệp Vụ Cốt Lõi (Core Business Processes)

```text
[Người dùng mở Web] ➔ [Cấp quyền Webcam] ➔ [Thực hiện Ký hiệu]
                                                   │
                                     (Hệ thống khoanh vùng & dịch)
                                                   │
[Phát âm thanh TTS] ◄── [Hiển thị câu hoàn chỉnh] ◄─┘
```

---

## 4. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)
* **Hiệu năng:** Độ trễ nhận diện < 100ms.
* **Bảo mật:** Luồng video không lưu trữ cố định trên server để bảo vệ quyền riêng tư người dùng.
* **Tương thích:** Chạy tốt trên Chrome, Edge, Safari, Firefox.