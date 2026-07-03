---
name: Quy chuẩn kiến trúc Backend FastAPI
description: Quy ước tổ chức dự án Backend sử dụng FastAPI.
tags:
  - backend
  - fastapi
  - architecture
paths:
  - "**/*.py"
---

# Quy chuẩn kiến trúc Backend FastAPI

## Mục tiêu

Tổ chức dự án rõ ràng, dễ mở rộng, dễ bảo trì và phù hợp với môi trường production.

---

## Nguyên tắc

- Mỗi thư mục chỉ có một trách nhiệm.
- API chịu trách nhiệm xử lý nghiệp vụ.
- Service chỉ thao tác với dữ liệu.
- Schema chỉ dùng để validate dữ liệu.
- Model chỉ ánh xạ cơ sở dữ liệu.
- Ưu tiên cấu trúc đơn giản, tránh tạo nhiều tầng không cần thiết.

---

# Cấu trúc dự án

```text
backend/
├── app/
│   ├── api/
│   │   └── routers/
│   │
│   ├── model/
│   │
│   ├── schemas/
│   │
│   ├── service/
│   │
│   ├── utils/
│   │
│   ├── config.py
│   ├── database.py
│   ├── security.py
│   └── main.py
│
├── migrations/
├── tests/
│
├── .env
├── .env.example
├── .dockerignore
├── Dockerfile
├── pyproject.toml
└── uv.lock
```

---

## app/api

Chứa toàn bộ API của hệ thống.

### routers

- Khai báo endpoint.
- Validate request.
- Xử lý nghiệp vụ.
- Gọi service.
- Trả response.

Không truy cập cơ sở dữ liệu trực tiếp.

Ví dụ

```text
routers/
├── auth.py
├── user.py
├── conversation.py
├── message.py
└── admin.py
```

---

## app/model

Chứa toàn bộ SQLModel ORM.

- Định nghĩa bảng.
- Quan hệ giữa các bảng.

Không chứa business logic hoặc truy vấn dữ liệu.

Ví dụ

```text
model/
├── user.py
├── conversation.py
├── message.py
├── document.py
└── user_memory.py
```

---

## app/schemas

Chứa toàn bộ Pydantic Schema.

- Request
- Response
- Validation

Không chứa business logic hoặc thao tác cơ sở dữ liệu.

Ví dụ

```text
schemas/
├── auth.py
├── user.py
├── conversation.py
└── message.py
```

---

## app/service

Chứa toàn bộ thao tác với dữ liệu.

Bao gồm

- CRUD
- Query
- Transaction

Không chứa business logic.

Ví dụ

```text
service/
├── auth_service.py
├── user_service.py
├── conversation_service.py
└── message_service.py
```

---

## app/utils

Chứa các hàm dùng chung.

Ví dụ

- Decorator
- File
- Retry
- Helper
- Validation

Không chứa business logic hoặc thao tác cơ sở dữ liệu.

---

## config.py

Quản lý cấu hình ứng dụng.

Bao gồm

- Pydantic Settings
- Environment Variables
- Constants

Không khai báo cấu hình ở nhiều nơi.

---

## database.py

Quản lý kết nối cơ sở dữ liệu.

Bao gồm

- Engine
- Session
- Metadata

Không chứa truy vấn nghiệp vụ.

---

## security.py

Quản lý bảo mật.

Bao gồm

- JWT
- Password Hash
- Authentication
- Authorization

---

## main.py

Điểm khởi động ứng dụng.

Bao gồm

- Khởi tạo FastAPI
- Middleware
- Router
- Exception Handler
- Startup
- Shutdown

Không chứa business logic.

---

## migrations

Chứa Alembic Migration.

- Version
- Migration Script

Không chỉnh sửa trực tiếp dữ liệu.

---

## tests

Chứa toàn bộ kiểm thử.

Bao gồm

- Unit Test
- Integration Test
- API Test
- End-to-End Test

Không đặt test trong thư mục `app`.

---

## Root

### pyproject.toml

Quản lý dependencies và cấu hình dự án.

### uv.lock

Khóa phiên bản package.

### Dockerfile

Build Docker Image.

### .dockerignore

Loại bỏ file không cần thiết khi build.

### .env.example

Mẫu biến môi trường.

Không chứa thông tin bí mật.

### .env

Biến môi trường của từng môi trường chạy.

Không commit lên Git.

---

## Không được phép

- Đặt truy vấn cơ sở dữ liệu trong API.
- Đặt business logic trong Service.
- Đặt business logic trong Model.
- Đặt business logic trong Schema.
- Hard-code thông tin cấu hình.
- Khai báo cấu hình ở nhiều nơi.
- Đặt tiện ích dùng chung vào Service.
- Đặt mã không liên quan trong cùng một module.