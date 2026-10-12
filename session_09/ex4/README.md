# BÀI 4: LƯU TRỮ DỮ LIỆU ĐƠN GIẢN VỚI DOCKER VOLUME

## 1. Mục tiêu
Tạo một Named Volume để lưu trữ tệp dữ liệu bền vững (persistence) giữa các lần chạy Container, chứng minh dữ liệu không bị mất khi container dừng và bị xóa bỏ.

---

## 2. Các bước thực hiện và Kết quả Terminal

### Bước 1: Tạo Named Volume mới
Tạo volume mang tên `easy-volume`:

```bash
docker volume create easy-volume
```

**Kết quả đầu ra (Terminal Output):**
```text
easy-volume
```

---

### Bước 2: Kiểm tra danh sách Volume
Kiểm tra danh sách các volume trên máy để đảm bảo `easy-volume` đã được khởi tạo:

```bash
docker volume ls
```

**Kết quả đầu ra (Terminal Output):**
```text
DRIVER    VOLUME NAME
local     docker_kafka_data
local     docker_redis_data
local     e5607c086a0251a4f2ce2948eae961436abf897040d9974b285836fbdd2ece74
local     easy-volume
```

---

### Bước 3: Ghi dữ liệu vào Volume bằng Container thứ nhất
Sử dụng một container Alpine tạm thời (tự động xóa sau khi chạy xong nhờ flag `--rm`), mount volume `easy-volume` vào thư mục `/data` và ghi nội dung vào file `/data/test.txt`:

```bash
docker run --rm -v easy-volume:/data alpine sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"
```

**Kết quả đầu ra (Terminal Output):**
```text
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
6d0606d1815c: Download complete
797dd00a0fc7: Download complete
Digest: sha256:294b683cb724975bec92580e1e685676bd4b50bda910ddb8c51d4cabeaec77e6
Status: Downloaded newer image for alpine:latest
```

*Nhận xét*: Sau khi ghi xong file `test.txt`, container đã hoàn thành nhiệm vụ và bị hủy hoàn toàn (`--rm`).

---

### Bước 4: Đọc lại dữ liệu bằng Container thứ hai (Kiểm tra)
Khởi chạy một container Alpine hoàn toàn mới, mount cùng volume `easy-volume` vào `/data` và đọc nội dung file `/data/test.txt`:

```bash
docker run --rm -v easy-volume:/data alpine cat /data/test.txt
```

**Kết quả đầu ra (Terminal Output):**
```text
Luu tru du lieu Docker
```

---

## 3. Kết luận
- Dữ liệu được ghi vào Named Volume `easy-volume` vẫn tồn tại nguyên vẹn ngay cả khi container đầu tiên đã kết thúc vòng đời và bị xóa bỏ.
- Container thứ hai có thể truy cập và đọc dữ liệu chính xác từ volume đó, chứng minh tính bền vững (data persistence) của Docker Volume.
