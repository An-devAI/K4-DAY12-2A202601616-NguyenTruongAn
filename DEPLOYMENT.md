# Thông Tin Deploy — Checkpoint 5

> Điền file này sau khi deploy xong. `pytest tests/test_cp5.py` đọc file này
> để tìm địa chỉ service của bạn và gọi thử.
>
> **Chỉ ghi TÊN biến môi trường, tuyệt đối không dán giá trị token vào đây.**
> Repo này công khai — dán token vào là mất token.

## Thông Tin Học Viên

| Mục | Nội dung |
|-----|----------|
| Họ và tên | Nguyễn Trường An |
| Mã học viên | 2A202601616 |
| Repo | https://github.com/An-devAI/K4-DAY12-2A202601616-NguyenTruongAn |

## Service

| Mục | Nội dung |
|-----|----------|
| Public URL | https://k4-day12-2a202601616-nguyentruongan.onrender.com/ |
| Platform | Render |
| Ngày deploy | 10/08/2026 |

## Biến Môi Trường Đã Set Trên Cloud

Ghi tên biến và **nguồn giá trị**, không ghi giá trị:

| Biến | Đã set | Ghi chú |
|------|--------|---------|
| `PORT` | ✅ | Render tự gán cho Web Service |
| `API_TOKEN` | ✅ | Đặt trong Render Environment, không nằm trong repo |
| `REDIS_URL` | ✅ | redis://red-d9sppjm417fc73b7l0e0:6379 |
| `BUCKET_CAPACITY` | x | 10 |
| `REFILL_PER_MINUTE` | x | 10 |
| `DAILY_BUDGET_USD` | x | 1.0 |
| `LOG_LEVEL` | x | INFO |

## Lệnh Kiểm Tra

Thay `<URL>` bằng Public URL ở trên:

```bash
# 1. Liveness — mong đợi 200 {"status":"ok"}
curl -i https://k4-day12-2a202601616-nguyentruongan.onrender.com/healthz

# 2. Readiness — mong đợi 200 {"status":"ready"} (đã nối được Redis)
curl -i https://k4-day12-2a202601616-nguyentruongan.onrender.com/readyz

# 3. Không có token — mong đợi 401 kèm header WWW-Authenticate
curl -i -X POST https://k4-day12-2a202601616-nguyentruongan.onrender.com/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello"}'

# 4. Có token — mong đợi 200 kèm câu trả lời
curl -i -X POST https://k4-day12-2a202601616-nguyentruongan.onrender.com/chat \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $API_TOKEN" \
  -H "X-Client-Id: sv-test" \
  -d '{"message":"Deploy là gì?"}'

# 5. Rate limit — gọi 15 lần, những lần cuối phải trả 429
for i in $(seq 1 15); do
  curl -s -o /dev/null -w "%{http_code} " -X POST https://k4-day12-2a202601616-nguyentruongan.onrender.com/chat \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $API_TOKEN" \
    -H "X-Client-Id: sv-test" \
    -d '{"message":"test"}'
done; echo
```

## Kết Quả Chạy Thật

Dán output của các lệnh trên vào đây:

```
an@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ curl -i https://k4-day12-2a202601616-nguyentruongan.onrender.com/healthz
HTTP/2 200 
date: Mon, 10 Aug 2026 10:23:15 GMT
content-type: application/json
rndr-id: b0641205-42cd-43a4
server: cloudflare
vary: Accept-Encoding
x-render-origin-server: uvicorn
cf-cache-status: DYNAMIC
cf-ray: a28e48b9db9c84b8-HKG
alt-svc: h3=":443"; ma=86400

an@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ curl -i https://k4-day12-2a202601616-nguyentruongan.onrender.com/readyz
HTTP/2 200 
date: Mon, 10 Aug 2026 10:23:59 GMT
content-type: application/json
rndr-id: 07fa67c9-5eb6-442e
server: cloudflare
vary: Accept-Encoding
x-render-origin-server: uvicorn
cf-cache-status: DYNAMIC
cf-ray: a28e49cbcc4afdfc-SIN
alt-svc: h3=":443"; ma=86400

an@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ curl -i -X POST https://k4-day12-2a202601616-nguyentruongan.onrender.com/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello"}'
HTTP/2 401 
date: Mon, 10 Aug 2026 10:26:09 GMT
content-type: application/json
rndr-id: a44c1d34-c19c-490d
server: cloudflare
vary: Accept-Encoding
www-authenticate: Bearer
x-render-origin-server: uvicorn
cf-cache-status: DYNAMIC
cf-ray: a28e4cfa7d61fd81-SIN
alt-svc: h3=":443"; ma=86400

{"detail":"invalid or missing bearer token"}an@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ curl -i -X POST https://k4-day12-2a202601616-nguan@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ curl -i -X POST https://k4-day12-2a202601616-nguyentruongan.onrender.com/chat \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $API_TOKEN" \
  -H "X-Client-Id: sv-test" \
  -d '{"message":"Deploy là gì?"}'
HTTP/2 200 
date: Mon, 10 Aug 2026 10:26:50 GMT
content-type: application/json
rndr-id: faf34ac8-7abb-4361
server: cloudflare
vary: Accept-Encoding
x-render-origin-server: uvicorn
cf-cache-status: DYNAMIC
cf-ray: a28e4dfb0df40865-HKG
alt-svc: h3=":443"; ma=86400

{"reply":"Câu hỏi hay. Deploy là gì thường được giải quyết bằng cách chuẩn hóa môi trường chạy: cùng một image chạy giống nhau ở laptop và trên cloud.","client_id":"sv-test","turns_before":0,"usd_cost":2.145e-05,"usage":{"prompt":3,"completion":35}}an@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ for i in $an@an-GF63-Thin-11UD:~/Downloads/labs/K4-DAY12-2A202601616-NguyenTruongAn$ for i in $(seq 1 15); do
  curl -s -o /dev/null -w "%{http_code} " -X POST https://k4-day12-2a202601616-nguyentruongan.onrender.com/chat \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $API_TOKEN" \
    -H "X-Client-Id: sv-test" \
    -d '{"message":"test"}'
done; echo
200 200 200 200 200 200 200 200 200 200 429 429 429 429 429 
```

## Ảnh Chụp Màn Hình

Đặt ảnh trong thư mục `screenshots/`:

- `screenshots/dashboard.png` — trang quản lý service trên platform
- `screenshots/healthz.png` — kết quả gọi `/healthz` từ trình duyệt hoặc curl

---

## Nếu Dùng Phương Án Dự Phòng

Không áp dụng vì đã deploy thành công lên Render.
