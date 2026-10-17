# 🚀 Deployment Guide: Enterprise WhatsApp AI Agent Stack

Dokumen ini menyediakan panduan langkah demi langkah untuk menginstalasi, mengonfigurasi, dan mengoperasikan stack **WhatsApp AI Agent** yang terdiri dari **n8n**, **WAHA (WhatsApp HTTP API)**, dan **OpenAI Integration** pada lingkungan server Cloud atau Docker Container.

---

## 📋 Prerequisites & System Requirements

Sebelum memulai deployment, pastikan infrastruktur server Anda memenuhi spesifikasi minimum berikut:

* **Compute:** Minimum 1 vCPU, 2 GB RAM (Direkomendasikan 2 vCPU, 4 GB RAM untuk menangani *traffic concurrency*).
* **OS:** Ubuntu 22.04 LTS / Debian 11+ atau layanan PaaS/Container Hosting (seperti Flaz Cloud, Railway, VPS AWS/GCP).
* **Dependencies:** Docker Engine Engine v24+, Docker Compose v2.20+, dan Domain Name dengan SSL/TLS Reverse Proxy (Nginx / Caddy).
* **API Accounts:**
  * OpenAI API Key (dengan kredit aktif untuk akses GPT Model).
  * Nomor WhatsApp khusus untuk koneksi gateway.

---

## 🏗️ Architecture Stack Deployment (Docker Compose)

Buat file `docker-compose.yml` di server Anda untuk menjalankan n8n dan WAHA secara bersamaan dalam satu jaringan lokal.

```yaml
version: '3.8'

services:
  # -----------------------------------------------
  # 1. WAHA Gateway (WhatsApp HTTP API)
  # -----------------------------------------------
  waha:
    image: devlikeapro/waha:latest
    container_name: waha_gateway
    restart: always
    environment:
      - WAHA_DASHBOARD_USERNAME=admin
      - WAHA_DASHBOARD_PASSWORD=YourSecurePassword123!
      - WHATSAPP_RESTART_ON_DISCONNECT=true
    ports:
      - "3000:3000"
    volumes:
      - waha_data:/app/.waha

  # -----------------------------------------------
  # 2. n8n Workflow Automation Engine
  # -----------------------------------------------
  n8n:
    image: docker.n8n.io/n8nio/n8n:latest
    container_name: n8n_engine
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=n8n.yourdomain.com
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=[https://n8n.yourdomain.com/](https://n8n.yourdomain.com/)
      - EXECUTIONS_DATA_PRUNE=true
      - EXECUTIONS_DATA_MAX_AGE=168
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  waha_data:
  n8n_data: