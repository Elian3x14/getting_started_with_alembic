# Tìm Hiểu Về Alembic – Thư Viện Migration Cho Python

Trong phát triển ứng dụng Python, đặc biệt khi dùng **SQLAlchemy**, việc quản lý **thay đổi cơ sở dữ liệu** là rất quan trọng. **Alembic** giúp tạo và quản lý migrations, đảm bảo schema của database luôn đồng bộ với code.

## Cài Đặt Alembic

Cài đặt Alembic:

```bash
pip install alembic
```

Tạo cấu trúc dự án Alembic:

```bash
alembic init alembic
```

Thư mục `alembic/` sẽ chứa file cấu hình và các migration script.

## Cấu Hình Alembic

Trong file `alembic.ini`, chỉnh sửa **database URL**:

```ini
sqlalchemy.url = postgresql://user:password@localhost/mydatabase
```

Các migration script sẽ được lưu trong thư mục `alembic/versions/`.

## Tạo Migration Script

Khi model thay đổi, tạo migration script:

```bash
alembic revision --autogenerate -m "Add dob column to user table"
```

Alembic sẽ so sánh model với database và sinh các lệnh SQL cần thiết.

## Áp Dụng Migration

Cập nhật database:

```bash
alembic upgrade head
```

Rollback về migration cũ:

```bash
alembic downgrade <revision_id>
```

## Ví Dụ Minh Họa

Model `User` ban đầu:

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    email = Column(String(120))
```

Khi muốn thêm cột `dob` (ngày sinh):

```python
from sqlalchemy import Date

dob = Column(Date)
```

Chỉ cần tạo migration script và chạy `alembic upgrade head`, cột `dob` sẽ được thêm vào bảng `users`.

## Những Lưu Ý Khi Dùng Alembic

* Luôn tạo migration script trước khi deploy, không chỉnh sửa schema trực tiếp trên production.
* Kiểm tra kỹ migration script sinh tự động, đặc biệt khi có các thay đổi phức tạp như rename column.
* Dùng môi trường test để chạy migration trước khi áp dụng trên production.
* Commit tất cả migration vào Git để đồng bộ với team.
* Không chỉnh sửa migration đã áp dụng; nếu cần thay đổi hãy tạo migration mới.
* Cẩn thận khi rollback vì có thể mất dữ liệu (ví dụ khi xóa column hoặc bảng).
* Hỗ trợ nhiều database, nhưng một số syntax SQL có thể khác nhau giữa các hệ quản trị.

## Kết Luận

Alembic giúp quản lý schema database hiệu quả, giảm lỗi và giữ cho database luôn đồng bộ với code. Đây là công cụ gần như không thể thiếu trong các dự án Python sử dụng SQLAlchemy, đặc biệt khi dự án phát triển nhanh và liên tục thay đổi cơ sở dữ liệu.
