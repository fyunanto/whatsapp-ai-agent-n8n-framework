# 🏛️ Architecture Overview: Enterprise WhatsApp AI Agent

Dokumen ini menjelaskan rancangan arsitektur, komponen utama, serta alur data dari sistem **WhatsApp AI Agent** berbasis **n8n Automation Engine**, **WAHA (WhatsApp HTTP API)**, dan **OpenAI LLM**.

---

## 🖼️ System Blueprint

![n8n Workflow Canvas](images/n8n-workflow-canvas.png)

---

## 🧩 Key Architecture Components

Sistem ini dirancang menggunakan pendekatan *decoupled microservices* berbasis kontainer untuk memastikan skala dan keandalan tinggi:

1. **Messaging Gateway (WAHA)**
   * Berfungsi sebagai jembatan antara protokol WhatsApp dan sistem backend via API HTTP.
   * Menangani *session management*, QR code authentication, serta konversi pesan masuk menjadi *webhook payload* JSON.
   * Dilengkapi fitur *Retry Policy* (Linear/Exponential backoff) untuk menjamin pengiriman webhook saat lalu lintas padat.

2. **Orchestration & Workflow Engine (n8n)**
   * Mengelola seluruh logika bisnis dan alur eksekusi pesan secara *event-driven*.
   * Menggunakan custom node `@devlikeapro/n8n-nodes-waha` untuk integrasi native dengan gateway WAHA.
   * Mengorkestrasi pemanggilan model AI dan pengelolaan state percakapan.

3. **Intelligence & Memory Layer (OpenAI Agent + Window Buffer Memory)**
   * **AI Agent Node:** Bertindak sebagai *decision maker* yang memproses perintah, menerapkan *system message/prompt*, dan mengarahkan output.
   * **OpenAI Chat Model:** Menggunakan model LLM (`gpt-4o` / `gpt-3.5-turbo`) untuk memahami konteks dan menghasilkan respons bahasa alami.
   * **Simple Memory (Window Buffer):** Mempertahankan konteks percakapan terakhir (10 riwayat pesan) berdasarkan ID unik pengirim (`payload.from`), sehingga interaksi terasa dinamis dan konsisten.

---

## 🔄 End-to-End Data Flow

```text
[ User WhatsApp ]
       │
       ▼ (1. Send Message)
[ WAHA Gateway ]
       │
       ▼ (2. HTTP Webhook Payload)
[ n8n Workflow ] ───► (3. Extract Prompt & Session ID)
       │
       ▼
[ LangChain AI Agent ]
       ├──► [ Simple Memory ] (4. Fetch Last 10 Context Messages)
       └──► [ OpenAI Model ]  (5. Process Request with System Prompt)
       │
       ▼ (6. Return Generated Response)
[ n8n Workflow ]
       │
       ▼ (7. Call Send Text API)
[ WAHA Gateway ]
       │
       ▼ (8. Deliver Message)
[ User WhatsApp ]
