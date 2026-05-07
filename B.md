# B.Cài đặt Ubuntu + Docker
# 1.Sử dụng VirtualBox để tạo máy ảo và cấu hình mạng cho Ubuntu và VirtualBox
# 2.Chuẩn bị trên Ubuntu Server
# Cài đặt và khởi động OpenSSH Server:
"sudo apt update"
"sudo apt install openssh-server -y"
"sudo systemctl enable --now ssh"
Sau khi chạy các lệnh cài đặt, khởi động xong => mở cổng firewall cho phép connect ssh:
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3e06e1b2-7bb4-4397-b278-f9684515c3cd" />
# Truy cập SSH từ Windows CMD:
Sử dụng lệnh:ssh admin@192.168.1.8
# <img width="766" height="825" alt="image" src="https://github.com/user-attachments/assets/d011d147-6e6a-4523-9e7c-79de9b0f1b92" />
# Tìm hiểu các lệnh trên Ubuntu:
--- 1. XEM VÀ ĐIỀU HƯỚNG THƯ MỤC ---
ls # Liệt kê file/thư mục cơ bản
ls -la # Xem chi tiết (kể cả file ẩn, dung lượng và quyền)
cd <đường_dẫn> # Đi tới thư mục (VD: cd /var/www)
cd .. # Lùi lại 1 thư mục cha
cd ~ # Trở về thư mục Home của user
mkdir <tên_thư_mục> # Tạo thư mục mới

--- 2. SAO CHÉP & CHỈNH SỬA FILE ---
cp <file_nguồn> <đường_dẫn_đích> # Sao chép file
cp -r <thư_mục_nguồn> <đường_dẫn_đích> # Sao chép toàn bộ thư mục (cần -r)
sudo nano <tên_file> # Mở file để sửa (Lưu: Ctrl+O -> Enter | Thoát: Ctrl+X)
--- 3. PHÂN QUYỀN FILE/THƯ MỤC ---
sudo chmod 777 <tên_file> # Cấp toàn quyền (Đọc/Ghi/Chạy) cho TẤT CẢ mọi người
sudo chmod 755 <tên_file> # Bạn có toàn quyền, người khác Đọc/Chạy
sudo chmod 644 <tên_file> # Bạn được Đọc/Ghi, người khác chỉ Đọc

--- 4. KIỂM TRA MẠNG ---
ip -4 addr # Xem địa chỉ IP (IPv4)
# 3.Cài đặt Docker (Sau khi đã Login thành công)
# B1: Khi màn hình hiện ra dòng chữ admin@Ubuntu-Server:~$
hãy dán lần lượt các lệnh này vào CMD:  
Cài đặt: sudo apt update && sudo apt install docker.io docker-compose-v2 -y.  
Kiểm tra phiên bản: docker --version và docker compose version
# B2: Kiểm tra phiên bản docker vừa cài đặt, kiểm tra phiên bản của docker compose
# Cấu hình để docker chạy mà không cần tiền tố sudo
# <img width="483" height="143" alt="image" src="https://github.com/user-attachments/assets/a1595831-bcf7-400a-a776-8c60ec619d35" />
# B3: Tìm hiểu tập lệnh của docker và docker compose
Với Docker
--- IMAGES ---
docker pull  # Tải image về máy
docker images # Xem danh sách image đã tải
docker rmi  # Xóa image

--- CONTAINERS ---
docker run -d -p 8080:80  # Tạo & chạy ngầm, map port (VD: nginx)
docker ps # Xem các container ĐANG CHẠY
docker ps -a # Xem TẤT CẢ container (cả đã tắt)
docker stop # Dừng container
docker start # Bật lại container đã dừng
docker rm # Xóa container (phải stop trước)
docker rm -f # Ép xóa ngay lập tức

--- DEBUG ---
docker logs # Xem log của container
docker exec -it bash # Chui vào bên trong container gõ lệnh (hoặc dùng 'sh')7

Với Docker Compose
docker compose up -d # Tự động tải & chạy tất cả dịch vụ ngầm
docker compose down # Dừng & xóa sạch các dịch vụ, network
docker compose ps # Xem trạng thái các dịch vụ trong cụm
docker compose logs -f # Xem log liên tục của tất cả dịch vụ
# B4: Đảm bảo tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
# <img width="578" height="505" alt="image" src="https://github.com/user-attachments/assets/d61c6d2c-c134-4cd0-99c0-e05575069683" />
