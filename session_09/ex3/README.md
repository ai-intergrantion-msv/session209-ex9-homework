# BÀI 3: VIẾT DOCKERFILE ĐÓNG GÓI TRANG WEB HTML TĨNH

## 1. Mục tiêu
Thực hành viết tệp Dockerfile ngắn gọn để đóng gói một trang web HTML tĩnh vào Docker Image Nginx, khởi chạy container và kiểm tra kết quả qua curl.

---

## 2. Cấu trúc thư mục
```text
ex3/
├── Dockerfile
├── index.html
└── README.md
```

---

## 3. Nội dung các tệp nguồn

### Tệp `index.html`:
```html
<h1>Hello Docker Session 09!</h1>
```

### Tệp `Dockerfile`:
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

---

## 4. Các bước thực hiện và Kết quả Terminal

### Bước 1: Build Docker Image (`docker build`)
Tiến hành build image với tag `my-html-app:v1` từ context hiện tại:

```bash
docker build -t my-html-app:v1 .
```

**Kết quả đầu ra (Terminal Output):**
```text
#0 building with "desktop-linux" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 104B 0.0s done
#1 DONE 0.1s

#2 [internal] load metadata for docker.io/library/nginx:alpine
#2 DONE 0.1s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [internal] load build context
#4 transferring context: 71B done
#4 DONE 0.0s

#5 [1/2] FROM docker.io/library/nginx:alpine@sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2
#5 resolve docker.io/library/nginx:alpine@sha256:df221db836e1754089190208cee7eeda94f233197056426eda74a43ab1abeac2 0.0s done
#5 DONE 0.2s

#6 [2/2] COPY index.html /usr/share/nginx/html/index.html
#6 DONE 0.0s

#7 exporting to image
#7 exporting layers 0.1s done
#7 exporting manifest sha256:c408397454311862d7e5fc6566454165dc1e29d9cf99d8b51bd0b0df9c291b3d 0.0s done
#7 exporting config sha256:6e2208ae9891bf5339d6ad5f8f3296316a1ed31e7bc371f6bda765d25af8d468 done
#7 exporting attestation manifest sha256:a71e35aec159db7c054e823c31b904da800c291360c0537783765e47f2d99702 0.0s done
#7 exporting manifest list sha256:29c735d232d8dfd16019b4992b80032048a2fa912a4181faab369a15e3c35070 0.0s done
#7 naming to docker.io/library/my-html-app:v1 done
#7 unpacking to docker.io/library/my-html-app:v1 0.1s done
#7 DONE 0.3s
```

---

### Bước 2: Khởi chạy container (`docker run`)
Khởi chạy container chạy ngầm (`-d`), đặt tên `html-app`, ánh xạ cổng `8081:80`:

```bash
docker run -d -p 8081:80 --name html-app my-html-app:v1
```

**Kết quả đầu ra (Terminal Output):**
```text
99622eb84407d25c09ba92ad06d638270d7aaafc27e69387ae0a9aea4b363617
```

Kiểm tra bằng `docker ps`:
```text
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                     NAMES
99622eb84407   my-html-app:v1   "/docker-entrypoint.…"   40 seconds ago   Up 39 seconds   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   html-app
```

---

### Bước 3: Kiểm tra phản hồi (`curl`)
Gửi HTTP request tới cổng `8081`:

```bash
curl http://localhost:8081
```

**Kết quả đầu ra (Terminal Output):**
```html
<h1>Hello Docker Session 09!</h1>
```

*Nhận xét*: Phản hồi chính xác nội dung HTML của trang web tĩnh đã đóng gói.

---

### Bước 4: Dọn dẹp container
Dừng và xóa container sau khi hoàn thành:

```bash
docker stop html-app && docker rm html-app
```

**Kết quả:**
```text
html-app
html-app
```

---

## 5. Kết luận
- Đóng gói thành công mã nguồn tĩnh vào image Nginx Alpine bằng Dockerfile ngắn gọn 2 dòng lệnh.
- Image build đạt chuẩn, kích thước tối ưu, chạy độc lập và phục vụ đúng nội dung web mong muốn.
