# YouTube Transcript Backend (Docker Compose)

โปรเจกต์นี้ใช้ Docker Compose สำหรับรันบริการ backend และเปิดใช้งานผ่าน Cloudflare Quick Tunnel

## Services

- `backend`
  - FastAPI backend
  - เปิดพอร์ต `8000` (`8000:8000`)
  - Health check: `http://localhost:8000/api/health`
  - ค่า image เริ่มต้น:
    - `alphabay0220/youtube-transcript-backend:latest`
  - สามารถ override ได้ด้วยตัวแปร `BACKEND_IMAGE`

- `cloudflared`
  - ใช้ `cloudflare/cloudflared:latest`
  - สร้าง Quick Tunnel ไปยัง `http://backend:8000`
  - รอจน backend healthy ก่อนเริ่มทำงาน

## Prerequisites

- ติดตั้ง Docker และ Docker Compose Plugin แล้ว

## Quick Start

1. (ทางเลือก) กำหนด backend image ที่ต้องการ

```bash
export BACKEND_IMAGE=alphabay0220/youtube-transcript-backend:latest
```

2. รันทุกบริการ

```bash
docker compose up -d
```

3. ตรวจสอบสถานะ

```bash
docker compose ps
```

4. ทดสอบ health endpoint ของ backend

```bash
curl http://localhost:8000/api/health
```

## Get Cloudflare Tunnel URL

Quick Tunnel จะเปลี่ยน URL ใหม่ทุกครั้งที่เริ่มรันใหม่ สามารถดู URL ล่าสุดได้จาก log:

```bash
docker compose logs -f cloudflared
```

มองหาข้อความที่มีโดเมน `trycloudflare.com`

## Useful Commands

- ดู log ทั้งหมด:

```bash
docker compose logs -f
```

- หยุดบริการ:

```bash
docker compose down
```

- รีสตาร์ท:

```bash
docker compose restart
```

## Environment Variables

- `BACKEND_IMAGE` (optional): ระบุ Docker image ของ backend

ตัวอย่าง:

```bash
BACKEND_IMAGE=myorg/youtube-transcript-backend:v1.0.0 docker compose up -d
```
