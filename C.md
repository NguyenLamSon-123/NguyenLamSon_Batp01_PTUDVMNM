# C. Cấu hình docker compose
## 1.Tạo thư mục: ~/lamson
## sử dụng mkdir ~/lamson để tạo thư mục
## Sau đó tạo tiếp các thư mục con bên trong
## mkdir myweb nginx nodered
# 2. Tạo file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3e84361f-37fa-4ddd-b134-1a5a246ce52f" />
## Cập nhật file nginx.conf
## nano ./nginx/nginx.conf
# <img width="498" height="447" alt="image" src="https://github.com/user-attachments/assets/7e8f6587-b86b-4327-8721-585dcf834882" />
# 3. Tạo lại file cấu hình
## nano docker-compose.yml
# <img width="478" height="499" alt="image" src="https://github.com/user-attachments/assets/d393117c-64bc-4112-899a-03f6ba3fecab" />
# Chạy lại lệnh Docker Compose
## docker compose up -d
# <img width="743" height="738" alt="image" src="https://github.com/user-attachments/assets/0f449221-e5f4-45a4-9d67-ba6bb818c128" />
# 4. Edit file ./nodered/settings.js để nodered bắt buộc đăng nhập
sau khi edit xong restart lại cho Nodred lưu 
# <img width="549" height="132" alt="image" src="https://github.com/user-attachments/assets/0392d234-3a34-4353-979f-36053cd01cca" />
