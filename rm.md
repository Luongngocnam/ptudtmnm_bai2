# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421

Lớp: 58KTPM

**Bài tập 02:** 
# SỬ DỤNG DJANGO ĐỂ TẠO WEB QUẢN LÝ TIỆM CẦM ĐỒ
## deadline : 23h59 ngày 09 tháng 5 năm 2026.
## Link gửi bài: [Tại đây](https://docs.google.com/spreadsheets/d/1zftQMj748nRpS-_br4_jdHZocNVvo848zqxCGcTy4uU)

1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)
   
2. SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ: 
- Mariadb  : chứa csdl của hệ thống này
- Phpmyadmin: để soi được csdl (chỉ để xem, ko cần tạo bảng từ đây, django sẽ làm hết)
- Django: build 1 docker container (dùng Dockerfile): trên nền python, sử dụng django, nhớ mount thư mục để dễ edit, edit dùng: sudo nano ten_file

sau khi có 3 service này trong file docker-compose.yml :
- run nó, cấu hình để Django nhận csdl mariadb (sửa file settings.py), cấu hình user login ban đầu, mô tả các bảng trong models.py, .... (đc phép sử dụng AI để làm) => KQ được trang admin, y/c đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng. các trường là khoá ngoại chỉ việc chọn text (mặc dù là csdl tại trường FK đó lưu ID của PK mà nó tham chiếu : sử dụng phpmyadmin để kiểm chứng)
- chú ý kết hợp ssh để chạy lệnh tác động vào django và sudo nano để edit file.
- sử dụng template (file html, sử dụng cú pháp jinja2), lấy context từ 1 view home_page, để tạo trang liệt kê các con nợ đến hạn mà chưa trả tiền!
- sử dụng cloudflare tunnel để public kết quả lên 1 sub-domain => chụp kết quả

Hướng dẫn:
- Tạo thư mục để chứa image tự buid cho django
- Vào thư mục đó tạo file tên: Dockerfile (nội dung hỏi AI xem file này cần có nội dung gì, full comment cho từng dòng lệnh trong file này => mục tiêu kép: để hiểu và để hệ thống chạy được)
- AI sẽ nói cần thêm file requirements.txt để cài các thư viện cho python (cài qua lệnh pip) => tạo file requirements.txt với nội dung tưng ứng, trong file này cũng comment được => comment xem thư viện nào dùng để làm gì
- Sau mỗi lần sửa đỏi có thể phải chạy lệnh dạng : **docker compose exec TÊN_SERVICE_DJANGO_CỦA_BẠN python manage.py migrate** để tác động vào django (còn nhiều lệnh khác chứ ko luôn như này), để django thay đổi csdl hoặc thay đổi cấu hình.

## CHUẨN BỊ MÔI TRƯỜNG DOCKER
Tạo một thư mục mới cho dự án mkdir pawn_shop && cd pawn_shop
Tạo file requirements.txt
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/d3397616-9c9c-41b2-9aaf-237d20e9f9e9" />
Tạo file Dockerfile (nano Dockerfile)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/c52ca728-34b7-4ff1-94e7-3bcd92b171ed" />
Tạo file docker-compose.yml để liên kết 3 services (MariaDB, PhpMyAdmin, Django).
<img width="1112" height="652" alt="image" src="https://github.com/user-attachments/assets/78e8ab61-09c9-4a16-966c-2282bd79c89b" />

## KHỞI TẠO DJANGO VÀ CẤU HÌNH CSDL
Build image và tạo project Django
```
docker compose build
docker compose run --rm web django-admin startproject config .
docker compose run --rm web python manage.py startapp core
```
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/45c57301-295b-45a6-b12c-acb4c4a32052" />
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/db090a6b-0c31-4494-9c7e-5550008f818c" />

Cấu hình kết nối MariaDB (sudo nano config/settings.py)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/7cca491b-350f-4f06-9aeb-a5f0a6905ea2" />

## TẠO MODELS & ADMIN QUẢN LÝ
Định nghĩa Models (sudo nano core/models.py)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/8fce55a8-4f4a-40d0-bad0-05609219e4c8" />
Đăng ký lên Admin (sudo nano core/admin.py)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/12e2d908-ec8e-432a-82c7-0f839708b094" />
Khởi chạy và Áp dụng CSDL
```
docker compose up -d
docker compose exec web python manage.py makemigrations
docker compose exec web python manage.py migrate
docker compose exec web python manage.py createsuperuser
```
<img width="1918" height="995" alt="image" src="https://github.com/user-attachments/assets/fd6b1a69-cc90-4394-8550-5aa3786e1952" />
Đăng nhập vào giao diện php
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/8b8ad5ca-1775-46c5-91f1-40cfaa1117e4" />
Đăng nhập vào django để thêm 1 vài con nợ 
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/b2405a1f-9b6c-4303-bcb8-59e94a49136b" />

## VIEW & TEMPLATE
Viết View (sudo nano core/views.py)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/4abcff49-c4ce-4eb9-8c42-c723deb9cc79" />
Tạo thư mục và file HTML (mkdir core/templates && sudo nano core/templates/home.html)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/37172c24-77a0-4973-b3a6-10ed206b01bb" />
Móc URL (sudo nano config/urls.py)
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/e7279810-685a-43f5-ab1d-32cedab6a847" />
## CHẠY CLOUDFLARE TUNNEL
Coppy 2 lệnh íntall chạy trong tẻminal
<img width="512" height="486" alt="image" src="https://github.com/user-attachments/assets/5f5006ec-c094-4f4f-9079-603e91644fef" />
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/06da9881-d292-482e-abc8-97f29c8abc12" />
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ab559787-86d7-4b04-b3b8-5197189d2c68" />

public tên miền thành công 
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/9485f52c-425b-45d1-8c71-a4cbf233a060" />

