# 04. Thiết Kế Kiến Trúc & Cơ Sở Dữ Liệu (SAD - System Architecture Document)

## 1. Sơ Đồ Kiến Trúc Hệ Thống (System Architecture)

```text
+-------------------------------------------------------+
|                   FRONTEND CLIENT                     |
|           React.js + HTML5 Canvas / WebSockets         |
+---------------------------┬---------------------------+
                            │ WebSocket Connection
                            ▼
+-------------------------------------------------------+
|                    BACKEND SERVICE                    |
|             FastAPI (Asynchronous Engine)             |
|                                                       |
|  +-------------------------------------------------+  |
|  |                YOLOv8 AI Engine                 |  |
|  |         (PyTorch / ONNX Runtime CPU/GPU)        |  |
|  +-------------------------------------------------+  |
+---------------------------┬---------------------------+
                            │ SQLAlchemy ORM
                            ▼
+-------------------------------------------------------+
|                   DATABASE LAYER                      |
|                PostgreSQL / SQLite                    |
+-------------------------------------------------------+
```

---

## 2. Thiết Kế Cơ Sở Dữ Liệu (Relational Database Design)

### Bảng `users`
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

### Bảng `translation_logs`
```sql
CREATE TABLE translation_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE SET NULL,
    detected_text TEXT NOT NULL,
    avg_confidence FLOAT NOT NULL,
    frame_count INT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

### Bảng `sign_dictionary`
```sql
CREATE TABLE sign_dictionary (
    id SERIAL PRIMARY KEY,
    class_id INT UNIQUE NOT NULL,
    sign_label VARCHAR(50) NOT NULL,
    meaning_description TEXT,
    example_image_url VARCHAR(255)
);
```