# Checklist Bài Nộp Day 12

> **Họ và tên:** ltlong  
> **Mã sinh viên:** 2A202600105  
> **Ngày:** 17/04/2026  
> **GitHub Repo:** https://github.com/ltlongg/Day12_2A202600105  
> **Nền tảng deploy:** Render  
> **Public URL:** https://ai-agent-8zg0.onrender.com

---

## Nguồn bằng chứng đã dùng

- Các ảnh chụp màn hình đã cung cấp cho thấy:
  - Deploy Render thành công và có public URL
  - Test local bằng `curl` với `X-API-Key`
  - Test `GET /ask` trả về `405 Method Not Allowed`
  - Test `POST /ask` trả về `200 OK`
  - Test rate limit trả về `429 Too Many Requests`
  - Terminal VS Code hiển thị nhiều request `POST /ask` và sau đó bị `429`
  - Trang `/docs` hiển thị các endpoint `/`, `/ask`, `/health`, `/ready`
- Các file trong repo đã được đối chiếu:
  - `06-lab-complete/`
  - `06-lab-complete/render.yaml`
  - `06-lab-complete/Dockerfile`
  - `06-lab-complete/docker-compose.yml`
  - `06-lab-complete/app/main.py`
  - `06-lab-complete/app/config.py`

---

## Xác minh triển khai

### Deploy trên Render

- [x] Service đã deploy thành công trên Render
- [x] Repo được kết nối: `ltlongg/Day12_2A202600105`
- [x] Branch deploy: `main`
- [x] Public URL hiển thị trên Render: `https://ai-agent-8zg0.onrender.com`
- [x] Ảnh chụp cho thấy:
  - First deploy started: `April 17, 2026 at 6:40 PM`
  - Deploy live: `April 17, 2026 at 6:42 PM`

### Xác minh API / Endpoint

- [x] `GET /ask` trả về `405 Method Not Allowed`
- [x] `POST /ask` hoạt động khi truyền đúng auth
- [x] API trả về JSON response hợp lệ
- [x] Rate limiting hoạt động và trả về `429 Too Many Requests`
- [x] `/docs` truy cập được ở local
- [x] Trang docs hiển thị các endpoint: `/`, `/ask`, `/health`, `/ready`

### Kết quả test xác nhận từ ảnh

#### 1. Test `curl` local khi thiếu auth

- Kết quả: request bị từ chối
- Response:

```json
{"detail":"Missing API key. Include header: X-API-Key: <your-key>"}
```

#### 2. Test `curl` local khi có API key

- Lệnh đã test trong terminal:

```bash
curl -H "X-API-Key: vinai_day12" http://localhost:8000/ask \
  -X POST -H "Content-Type: application/json" \
  -d '{"question": "hello"}'
```

- Kết quả: thành công
- Response theo ảnh:

```json
{
  "question": "hello",
  "answer": "Đây là câu trả lời từ AI agent (mock). Trong production, đây sẽ là response từ OpenAI/Anthropic."
}
```

#### 3. Test `GET /ask` bằng API client

- Status: `405 Method Not Allowed`
- Response:

```json
{
  "detail": "Method Not Allowed"
}
```

#### 4. Test `POST /ask` bằng API client

- Status: `200 OK`
- Thông tin request trong ảnh:
  - URL: `http://localhost:8000/ask`
  - Method: `POST`
  - Auth: Bearer Token
- Response theo ảnh:

```json
{
  "question": "what is docker?",
  "answer": "Container là cách đóng gói app để chạy ở mọi nơi. Build once, run anywhere!",
  "usage": {
    "requests_remaining": 9,
    "budget_remaining_usd": 1.9e-05
  }
}
```

#### 5. Test rate limit

- Status: `429 Too Many Requests`
- Response theo ảnh:

```json
{
  "detail": {
    "error": "Rate limit exceeded",
    "limit": 10,
    "window_seconds": 60,
    "retry_after_seconds": 37
  }
}
```

- Terminal trong VS Code cũng cho thấy nhiều request `POST /ask` thành công trước khi bị `429 Too Many Requests`.

---

## Kiểm tra source code

### Trạng thái repo / deliverable

- [x] Repo GitHub đã tồn tại
- [x] Thư mục gốc chứa đầy đủ các phần lab Day 12
- [x] Có thư mục `06-lab-complete/`
- [x] Có file `06-lab-complete/render.yaml`
- [x] Có file `06-lab-complete/Dockerfile`
- [x] Có file `06-lab-complete/docker-compose.yml`
- [x] Có file `06-lab-complete/.env.example`
- [x] Có file `06-lab-complete/.dockerignore`
- [x] Có file `06-lab-complete/README.md`
- [ ] Chưa thấy `MISSION_ANSWERS.md`
- [ ] Chưa thấy `DEPLOYMENT.md`

### Các file ứng dụng thực tế tìm thấy

Các file đã xác nhận trong `06-lab-complete/app/`:

- [x] `main.py`
- [x] `config.py`
- [ ] Chưa thấy `auth.py` trong `06-lab-complete/app/`
- [ ] Chưa thấy `rate_limiter.py` trong `06-lab-complete/app/`
- [ ] Chưa thấy `cost_guard.py` trong `06-lab-complete/app/`

Ghi chú:

- Logic security, rate limiting, cost guard, health, readiness và graceful shutdown hiện đang được viết trực tiếp trong `06-lab-complete/app/main.py`.
- Các file module tách riêng như `auth.py`, `rate_limiter.py`, `cost_guard.py` có tồn tại trong `04-api-gateway/production/`, nhưng không nằm trong `06-lab-complete/app/`.

### Các tính năng production đã xác minh trong code

- [x] Dockerfile nhiều stage
- [x] Có endpoint health check: `GET /health`
- [x] Có endpoint readiness check: `GET /ready`
- [x] Có xác thực API key qua `X-API-Key`
- [x] Có rate limiting
- [x] Có cost guard
- [x] Cấu hình lấy từ environment variables
- [x] Có graceful shutdown handler
- [x] Có security headers
- [x] Có CORS middleware
- [x] Có structured logging
- [x] Docker chạy bằng non-root user
- [x] Có file cấu hình Render
- [ ] Chưa phải rate limiting/stateless hoàn toàn bằng Redis trong `06-lab-complete/app/main.py`

### Lưu ý quan trọng về cấu hình

Có một điểm chưa khớp giữa ảnh test và cấu hình mặc định trong `06-lab-complete`:

- Ảnh test cho thấy rate limit là `10 req/min`
- `06-lab-complete/.env.example` và `06-lab-complete/render.yaml` đang để mặc định `20 req/min`

Điều này cho thấy app bạn test local và template cuối trong `06-lab-complete` có thể chưa dùng đúng cùng một cấu hình.

---

## Đối chiếu với yêu cầu nộp bài

### 1. Mission Answers

- [ ] Chưa có file `MISSION_ANSWERS.md`
- Khuyến nghị: cần tạo file này trước khi nộp

### 2. Full Source Code

- [x] Source code đã có trong repo
- [x] Các file Docker và deploy đã có
- [x] Các tính năng API cốt lõi đã được triển khai
- [ ] Cấu trúc thư mục cuối chưa khớp hoàn toàn với checklist mẫu ban đầu

### 3. Thông tin domain service

- [x] Đã xác định được public URL: `https://ai-agent-8zg0.onrender.com`
- [x] Đã xác định nền tảng deploy: Render
- [ ] Chưa có file `DEPLOYMENT.md`

---

## Checklist trước khi nộp

- [x] Đã có URL repo
- [ ] Chưa xác minh được repo đang public chỉ từ ảnh
- [ ] `MISSION_ANSWERS.md` đã hoàn thành
- [ ] `DEPLOYMENT.md` đã hoàn thành
- [x] Có `README.md`
- [x] Có `.env.example`
- [x] Không thấy commit file `.env` trong thư mục gốc hoặc `06-lab-complete/`
- [x] Chưa thấy hardcoded production secret rõ ràng trong các file đã kiểm tra
- [x] Public URL có hiển thị trong ảnh deploy
- [ ] Chưa test trực tiếp public URL từ bằng chứng hiện có
- [ ] Chưa thấy thư mục `screenshots/` trong repo
- [ ] Chưa xác minh lịch sử commit rõ ràng

---

## Các file nên bổ sung trước khi nộp

### `MISSION_ANSWERS.md`

Nên có các phần:

- Part 1: Localhost vs Production
- Part 2: Docker
- Part 3: Cloud Deployment
- Part 4: API Security
- Part 5: Scaling & Reliability

### `DEPLOYMENT.md`

Nên có các nội dung:

- Public URL: `https://ai-agent-8zg0.onrender.com`
- Platform: `Render`
- Lệnh health check
- Lệnh test API có auth
- Các environment variables đã dùng
- Link hoặc ảnh chụp màn hình

---

## Lệnh gợi ý để tự kiểm tra

```bash
curl https://ai-agent-8zg0.onrender.com/health
```

```bash
curl -X POST https://ai-agent-8zg0.onrender.com/ask \
  -H "X-API-Key: YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"question":"Hello"}'
```

---

## Kết luận hiện tại

Các bằng chứng hiện có cho thấy project của bạn đã có:

- deploy Render thành công
- API hoạt động
- xác thực request
- rate limiting
- docs page
- endpoint health/readiness trong code

Trước khi nộp chính thức, các mục còn thiếu chính là:

- `MISSION_ANSWERS.md`
- `DEPLOYMENT.md`
- đưa ảnh chụp vào repo
- xác minh lại repo public và public URL hoạt động trực tiếp

---

## Danh sách ảnh tham chiếu

- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 184334.png`
- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 224242.png`
- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 230620.png`
- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 230830.png`
- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 230853.png`
- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 230907.png`
- `c:/Users/Admin/Pictures/Screenshots/Screenshot 2026-04-17 231851.png`
