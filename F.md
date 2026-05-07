# F.Câu hỏi về bài làm?
## 1. Tại sao dùng Nginx làm Reverse Proxy thay vì trỏ thẳng Tunnel vào Node-RED?
Sơn có thể tưởng tượng Nginx là một ông bảo vệ ở cổng, còn Node-RED là văn phòng làm việc:
Quản lý tập trung: Nếu sau này Sơn có thêm Dashboard 2, Grafana, hay Database... Nginx sẽ điều phối tất cả qua các đường dẫn khác nhau như /api, /monitoring. Nếu dùng Tunnel trỏ thẳng, Sơn sẽ phải tạo rất nhiều Tunnel hoặc cấu hình cực kỳ phức tạp.

Bảo mật & Hiệu năng: Nginx có khả năng nén dữ liệu (Gzip), đệm (Caching) và chặn các cuộc tấn công cơ bản trước khi chúng chạm tới Node-RED.

SSL Termination: Nginx xử lý chứng chỉ bảo mật cực tốt, giúp giảm tải cho các ứng dụng phía sau.

## 2. Sự khác biệt giữa Mount file và Mount thư mục
Mount File: Chỉ gắn duy nhất 1 file từ Ubuntu vào Container. Thường dùng cho file cấu hình cụ thể (như nginx.conf).

Nhược điểm: Nếu Sơn dùng các trình soạn thảo như vim để sửa file trên Ubuntu, inode của file có thể thay đổi và Docker sẽ mất kết nối với file đó trong container.

Mount Thư mục: Gắn cả một cái kho. Tất cả những gì trong thư mục Ubuntu sẽ hiện ra trong Container.

Ưu điểm: Linh hoạt hơn, ổn định hơn khi chỉnh sửa nội dung bên trong. Thường dùng cho thư mục chứa web (/html).

## 3. Thay đổi index.html có thay đổi ngay trên web không?
Nếu Sơn dùng docker cp: Câu trả lời là Không. Sơn phải chạy lại lệnh docker cp mỗi khi sửa xong.

Nếu Sơn dùng Volume Mount (-v): Câu trả lời là Có, nhưng có thể bị trễ vài giây do cache của trình duyệt hoặc Nginx.

Tại sao? Vì Nginx đọc file từ ổ đĩa của container. docker cp chỉ chép file vào tại một thời điểm, còn Mount là tạo một "đường ống" dẫn trực tiếp dữ liệu từ Ubuntu vào container.

## 4. Restart Policy: always vs unless-stopped
restart: always: Container sẽ luôn tự khởi động lại nếu bị lỗi, hoặc khi Sơn khởi động lại máy Ubuntu. Kể cả khi Sơn chủ động tắt nó bằng lệnh docker stop, khi khởi động lại Docker Daemon, nó vẫn sẽ "sống dậy".

restart: unless-stopped: Tương tự như always, nhưng có một điểm khác biệt quan trọng: Nếu Sơn chủ động chạy lệnh docker stop, container sẽ nằm im và không tự chạy lại khi máy Ubuntu khởi động. Nó "biết nghe lời" hơn.

## 5. Chung 1 Network: Cách làm & Lợi ích
Lợi ích:

DNS nội bộ: Các service gọi nhau bằng tên (ví dụ: Node-RED gọi mysql-db) thay vì dùng địa chỉ IP.

Bảo mật: Các service trao đổi dữ liệu với nhau trong "vùng kín", không cần phơi bày ra ngoài internet.

Sửa đổi docker-compose.yml:

YAML
services:
  nginx-lamson:
    image: nginx:latest
    networks:
      - sonlam_network
  nodered-lamson:
    image: nodered/node-red:latest
    networks:
      - sonlam_network

networks:
  sonlam_network:
    driver: bridge
## 6. Bảo mật với .env và .gitignore
Sơn tạo file .env chứa: CF_TOKEN=your_token_here. Sau đó trong docker-compose.yml, Sơn gọi ${CF_TOKEN}.

Tại sao phải dùng .gitignore? Vì GitHub là nơi công cộng. Nếu Sơn đẩy Token lên đó, hacker sẽ lấy được và chiếm quyền điều khiển Cloudflare của bạn.

Tầm quan trọng: Đây là nguyên tắc "Separation of Config from Code" (Tách biệt cấu hình và mã nguồn). Code có thể chia sẻ, nhưng "chìa khóa" (Token) thì phải giữ ở nhà (máy Ubuntu).

## 7. Tại sao nên dùng hậu tố :ro?
:ro là viết tắt của Read-Only (Chỉ đọc).

Lý do: Ngăn container chỉnh sửa file gốc trên Ubuntu của Sơn. Nếu container của Sơn bị hacker chiếm quyền, chúng cũng không thể sửa đổi file cấu hình hệ thống để tạo "cửa sau" (Backdoor). Đây là tiêu chuẩn bảo mật cao nhất cho SRE.

## 8. Cloudflare Tunnel: Có cần mở cổng (Port) không? (Dành cho tiểu luận)
Đây là phần lý thuyết cốt lõi cho bài của Sơn:

Không cần thiết phải mở cổng (Inbound Port) trên Router/Firewall.

giải thích:

Cơ chế Outbound Connection: Cloudflare Tunnel hoạt động theo cơ chế kết nối ngược. Container cloudflared trên máy Ubuntu sẽ chủ động thiết lập một kết nối đi ra (Outbound) tới server của Cloudflare qua cổng 443.

Reverse Tunneling: Vì là kết nối đi ra, nên Firewall của Ubuntu mặc định sẽ cho phép. Cloudflare sau đó sẽ "mượn" đường ống này để gửi dữ liệu người dùng từ internet đi ngược vào trong máy của Sơn.

Lợi ích tiểu luận: * Giảm diện tích tấn công (Attack Surface): Hacker không thể quét (Scan) thấy bất kỳ cổng mở nào trên IP nhà Sơn.

Vượt qua NAT: Không cần cấu hình Port Forwarding phức tạp trên Modem nhà mạng.

Bảo mật IP: Địa chỉ IP thật của máy Ubuntu hoàn toàn được ẩn giấu sau lớp khiên của Cloudflare.
