## Trong Cloudflare: Tạo tunnel (đường hầm), chọn loại triển khai cho docker 
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/95bd9f6c-3362-4dab-85e5-a6f06741e960" />
## Convert lệnh docker run ... sang dạng docker compose và Khai báo kết quả convert vào trong file docker-compose.yml
Chỉnh sửa file:
mở file: nano ~/lamson/docker-compose.yml
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6d37d6f-d9a0-4ae8-a0d2-0f0e9878eb3d" />
bổ sung <br>
  tunnel: <br>
    image: cloudflare/cloudflared:latest <br>
    container_name: cloudflare-tunnel <br>
    restart: always <br>
    command: tunnel --no-autoupdate run --token ( cThem cụm <_MÃ_TOKEN_VÀO_ĐÂY> bằng chuỗi ký tự dài mà bạn lấy được từ Cloudflare (phần chọn Docker) <br>
# chạy lại docker compes
## <img width="474" height="100" alt="image" src="https://github.com/user-attachments/assets/bf76148e-b350-47e9-84c1-ea73f1985f4c" />
# Public ứng dụng bằng cách thêm 1 router trỏ tới container đang chạy trong docker, dữ liệu sẽ đi qua tunnel, url dạng sub-domain
cấu hình:
## <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b20e879-acf9-4310-aa2b-5e9aa228fc4e" />
## <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d18939b5-cdd3-43dc-8545-eb583dfedd46" />
## <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5fd3f432-3dee-4071-875e-40ec5d2fe492" />
