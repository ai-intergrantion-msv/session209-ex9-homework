# BÀI 2: KHỞI CHẠY WEB SERVER NGINX VỚI PORT MAPPING

## 1. Mục tiêu
Thực hành khởi chạy một Web Server Nginx dạng container ở chế độ chạy ngầm (`-d`), ánh xạ cổng mạng (`-p`), kiểm tra dịch vụ qua `curl`, và dọn dẹp container sau khi hoàn thành.

---

## 2. Các bước thực hiện và Kết quả Terminal

### Bước 1: Khởi chạy container Nginx
Khởi chạy container từ image `nginx:alpine`, đặt tên container là `my-web`, chạy ở chế độ nền (detached `-d`) và ánh xạ cổng `8080` trên máy chủ tới cổng `80` của container (`-p 8080:80`):

```bash
docker run -d -p 8080:80 --name my-web nginx:alpine
```

**Kết quả đầu ra (Terminal Output):**
```text
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
e72112c14215: Pulling fs layer
745dfb2690dd: Pulling fs layer
64c8194480fe: Pulling fs layer
d9aae54b5831: Pulling fs layer
e76228b47809: Pulling fs layer
e2de96513ba9: Pulling fs layer
9a9a644fdd6a: Pulling fs layer
6c53d0b2a666: Pulling fs layer
745dfb2690dd: Download complete
64c8194480fe: Download complete
d9aae54b5831: Download complete
6c53d0b2a666: Download complete
e76228b47809: Download complete
e2de96513ba9: Download complete
9a9a644fdd6a: Download complete
e72112c14215: Download complete
e2de96513ba9: Pull complete
0deea27e58d9: Download complete
d54d3939625e: Download complete
745dfb2690dd: Pull complete
d9aae54b5831: Pull complete
6c53d0b2a666: Pull complete
9a9a644fdd6a: Pull complete
64c8194480fe: Pull complete
e76228b47809: Pull complete
e72112c14215: Pull complete
Digest: sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2
Status: Downloaded newer image for nginx:alpine
bfdb0dc19517f22228f7bc84bac47827f0b666020b26a1b75a3d1bd3ef71f1d7
```

---

### Bước 2: Kiểm tra container đang chạy (`docker ps`)
Kiểm tra danh sách các container đang hoạt động để xác nhận container `my-web` đã khởi chạy và cổng `8080:80` đã được ánh xạ thành công:

```bash
docker ps
```

**Kết quả đầu ra (Terminal Output):**
```text
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
bfdb0dc19517   nginx:alpine   "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
```

---

### Bước 3: Gửi truy vấn kiểm tra trang web (`curl`)
Gửi yêu cầu HTTP GET tới địa chỉ `http://localhost:8080` để kiểm tra phản hồi từ web server:

```bash
curl http://localhost:8080
```

**Kết quả đầu ra (Terminal Output):**
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

*Nhận xét*: Trang web phản hồi thành công với tiêu đề và thẻ `<h1>` có nội dung `"Welcome to nginx!"`.

---

### Bước 4: Dọn dẹp container (`docker stop` & `docker rm`)
Dừng và xóa container `my-web` sau khi thử nghiệm xong:

```bash
docker stop my-web && docker rm my-web
```

**Kết quả đầu ra (Terminal Output):**
```text
my-web
my-web
```

Kiểm tra lại danh sách container bằng `docker ps -a`:
```bash
docker ps -a
```
Container `my-web` đã được dọn dẹp sạch sẽ khỏi hệ thống.

---

## 3. Kết luận
- Khởi chạy thành công Nginx Web Server ở chế độ detached (`-d`) sử dụng image `nginx:alpine`.
- Ánh xạ cổng (Port mapping `-p 8080:80`) hoạt động chuẩn xác, truy cập từ máy chủ qua `http://localhost:8080` nhận được nội dung mặc định của Nginx.
- Tiến hành dừng và xóa container thành công, giải phóng tài nguyên hệ thống.
