# RUN_LOCAL.md – Hướng dẫn chạy Lab 04

Tài liệu này giúp người khác clone repo sạch và chạy lại service trong Docker.

---

## 1. Clone repo

```bash
git clone <https://github.com/FIT-17-13/fit4110-lab04-nhom2_wl_aivision.git>
cd fit4110-lab04-nhom2_wl_aivision
```

---

## 2. Cài dependencies cho Newman/Prism/Spectral

```bash
npm install newman newman-reporter-html --legacy-peer-deps
```

---

## 3. Build Docker image

```bash
docker build -t fit4110/ai-vision:lab04 .
```

---

## 4. Run container

```bash
docker run --rm \
  --name fit4110-aivision-lab04 \
  -p 8000:8000 \
  --env-file .env.example \
  fit4110/ai-vision:lab04
```

Mở terminal khác, kiểm tra:

```bash
curl http://localhost:8000/health
```

Kết quả mong đợi:

```json
{
  "status": "ok",
  "service": "AI Vision (YOLOv8 active)",
  "timestamp": "2026-06-03T12:00:00.000000+00:00"
}
```

---

## 5. Chạy Newman test trên container

```bash
npx newman run postman/collections/FIT4110_lab03_ai_vision.postman_collection.json \
  --env-var "baseUrl=http://localhost:8000" \
  --env-var "cameraStreamMockUrl=[https://httpbin.org/anything](https://httpbin.org/anything)" \
  -r "cli,html,junit" \
  --reporter-html-export reports/newman-lab04-local.html \
  --reporter-junit-export reports/newman-lab04-local.xml
```

Report sinh tại:

```text
reports/newman-lab04-local.xml
reports/newman-lab04-local.html
```

---

## 6. Dừng container

Nếu không dùng `--rm` hoặc container còn chạy:

```bash
docker stop fit4110-aivision-lab04
```

---

## 7. Lệnh nhanh

```bash
make build
make run
make test-docker
make stop
```
