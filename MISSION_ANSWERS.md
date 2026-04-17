# Bài Lab 12 - Đáp Án

## Phần 1: Localhost và Production

### Câu 1.1: Các anti-pattern đã phát hiện
1. API key bị hardcode trực tiếp trong code thay vì đọc từ biến môi trường.
2. Không có endpoint health check để nền tảng giám sát trạng thái ứng dụng.
3. Cấu hình debug/phát triển được bật mặc định.
4. Ứng dụng không xử lý SIGTERM một cách graceful.
5. Cấu hình bị rải rác hoặc cố định trong code thay vì được quản lý tập trung.

### Câu 1.3: Bảng so sánh
| Tính năng | Môi trường phát triển | Production | Vì sao quan trọng? |
|---------|---------|------------|----------------|
| Cấu hình | Hardcode hoặc chỉ dùng cục bộ | Đọc từ biến môi trường qua `config.py` | Giúp giữ bí mật ngoài code và dễ triển khai ở nhiều nơi |
| Bí mật | Viết trực tiếp trong code hoặc `.env` | Secret manager / biến môi trường triển khai | Tránh rò rỉ và tránh commit nhầm |
| Cổng | Thường cố định khi chạy local | Lấy từ biến `PORT` | Cần thiết cho các nền tảng quản lý ứng dụng |
| Health check | Thường không có | `GET /health` và `GET /ready` | Hỗ trợ giám sát và rollout an toàn |
| Logging | `print()` hoặc log rời rạc | Structured JSON logging | Dễ tìm kiếm, tổng hợp và cảnh báo hơn |
| Shutdown | Tắt ngay khi dừng process | Graceful shutdown khi nhận SIGTERM | Tránh làm rơi request đang xử lý |
| Scale | Một instance, state nằm trong RAM | Stateless với state lưu bằng Redis | Hỗ trợ scale ngang |

## Phần 2: Docker

### Câu 2.1: Câu hỏi về Dockerfile
1. Base image: `python:3.11-slim` cho stage chạy production.
2. Working directory: `/app`.
3. Thứ tự cài dependency: copy `requirements.txt` trước, chạy `pip install`, sau đó mới copy mã nguồn ứng dụng.
4. Mô hình user: chạy bằng user không phải root là `agent`.
5. Kiểm tra sức khỏe: dùng Docker `HEALTHCHECK` cho endpoint `/health`.

### Câu 2.3: So sánh kích thước image
- `agent-develop`: `1.66 GB`
- `production-agent`: `236 MB`
- Chênh lệch: khoảng `85.8%` nhỏ hơn

### Giải thích so sánh kích thước image
- Image `agent-develop` lớn hơn nhiều vì dùng base image đầy đủ và ít tối ưu hơn.
- Image `production-agent` nhỏ hơn rõ rệt nhờ multi-stage build và `python:3.11-slim`.
- Theo ảnh kiểm tra, `agent-develop` là `1.66GB`, còn `production-agent` là `236MB`.

## Phần 3: Triển khai Cloud

### Câu 3.1: Triển khai Railway
- URL: https://lab12-production-e0585.up.railway.app
- Ảnh chụp màn hình: [Railway dashboard](screenshots/railway-dashboard.png)
- Ảnh chụp màn hình: [Railway health check](screenshots/railway-health.png)

## Phần 4: Bảo mật API

### Câu 4.1-4.3: Kết quả kiểm thử
Bảo mật bằng API key hoạt động đúng như mong đợi.

Ví dụ request thành công:
```text
API_KEY=$(grep AGENT_API_KEY .env | cut -d= -f2)
curl -H "X-API-Key: $API_KEY" \
     -X POST http://0.0.0.0:8000/ask \
     -H "Content-Type: application/json" \
     -d '{"question": "What is deployment?"}'

{"question":"What is deployment?","answer":"Deployment là quá trình đưa code từ máy bạn lên server để người khác dùng được.","model":"gpt-4o-mini","timestamp":"2026-04-17T14:42:40.300452+00:00"}
```

Ví dụ khi thiếu key:
```text
{"detail":"Invalid or missing API key. Include header: X-API-Key: <key>"}
```

### Câu 4.4: Cách triển khai cost guard
Cost guard dùng cách ước lượng token nhẹ dựa trên số từ của input và output, đổi ra mức tiêu thụ token xấp xỉ, rồi theo dõi tổng chi phí mỗi ngày trong bộ nhớ. Nếu vượt ngân sách ngày, ứng dụng sẽ từ chối request với mã 503. Cách này giúp agent không tiếp tục tiêu tiền sau khi chạm giới hạn.

## Phần 5: Mở rộng và Độ tin cậy

### Câu 5.1-5.5: Ghi chú triển khai
1. Dịch vụ được thiết kế theo hướng stateless ở tầng ứng dụng.
2. Trạng thái hội thoại/session được lưu trong Redis thay vì trong bộ nhớ của process.
3. Endpoint health và readiness giúp orchestrator quyết định khi nào ứng dụng sẵn sàng phục vụ.
4. `test_stateless.py` chứng minh rằng các request vẫn chạy được dù được phục vụ bởi nhiều instance khác nhau.
5. Graceful shutdown được triển khai để request đang xử lý có thể hoàn tất trước khi tắt.
6. Structured logging giúp gỡ lỗi và quan sát hệ thống dễ hơn khi chạy nhiều instance.