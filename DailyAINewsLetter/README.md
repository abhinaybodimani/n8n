# Daily Newsletter to Mail/Whatsapp Automation

Automated pipeline built with **n8n** that curates daily Artificial Intelligence news from Google News RSS, summarizes and formats the top stories with **Google Gemini**, and broadcasts the digest simultaneously via **Email (HTML)** and **WhatsApp (WAHA)**.

---

## 🏗️ Architecture

```
[Schedule Trigger] 
       │
       ▼
[Read config.json] ──► [Extract JSON] ──► [Google News RSS] ──► [Set Limit]
                                                                      │
                                                                      ▼
                                                            [Aggregate Items]
                                                                      │
                                                                      ▼
                                                           [Gemini LLM Chain]
                                                                      │
                                                                      ▼
                                                            [Parse AI Output]
                                                                      │
                                                                      ▼
                                                            [Distribute List]
                                                               ┌──────┴──────┐
                                                               ▼             ▼
                                                        [Email Filter] [WA Filter]
                                                               │             │
                                                               ▼             ▼
                                                         [Send Email]  [WAHA API]
```

---

## ⚙️ Tech Stack

* **Workflow Engine:** n8n (Self-Hosted via Docker)
* **LLM:** Google Gemini (`gemini-3.5-flash-lite-latest`)
* **WhatsApp Gateway:** WAHA (WhatsApp HTTP API)
* **Feed Source:** Google News RSS
* **Configuration:** Host-mounted `config.json`

---

## 📁 Configuration (`config.json`)

Subscriber data and runtime settings are stored in an external file so contact lists can be updated without modifying the workflow canvas.

Save this file locally at `C:\MyWork\SharedFolder4_n8n\DailyAINews\config.json`:

```json
{
  "data": {
    "settings": {
      "article_count": 7
    },
    "subscribers": [
      {
        "email": "user1@example.com",
        "whatsapp": "1234567890@c.us"
      },
      {
        "email": "user2@example.com",
        "whatsapp": "0987654321@c.us"
      },
      {
        "whatsapp": "1122334455@c.us"
      }
    ]
  }
}
```

---

## 🚀 Docker Setup & Deployment

### 1. Download Docker Images

Pull the latest required images to your local machine:

```powershell
docker pull docker.n8n.io/n8nio/n8n
docker pull devlikeapro/waha
```

### 2. Start n8n Container

Run this in PowerShell on Windows to mount your local folder to `/data` and enable local file access:

```powershell
docker run -d `
  --name n8n `
  -p 5678:5678 `
  -e NODE_FUNCTION_ALLOW_BUILTIN_EXTERNAL=* `
  -e N8N_DEFAULT_BINARY_DATA_MODE=filesystem `
  -e N8N_RESTRICT_FILE_ACCESS_TO=/data `
  -v n8n_storage:/home/node/.n8n `
  -v C:/MyWork/SharedFolder4_n8n:/data `
  docker.n8n.io/n8nio/n8n
```

### 3. Start WAHA (WhatsApp Gateway)

```powershell
docker run -d `
  --name waha `
  -p 3000:3000 `
  -e WAHA_API_KEY=YOUR_API_KEY `
  -v waha_data:/app/.sessions `
  devlikeapro/waha
```

---

## 📥 Import the Workflow

1. Open n8n at `http://localhost:5678`.
2. Click **Add workflow** -> **...** (top right) -> **Import from file**.
3. Select `DailyAINewsletter.json`.
4. Configure credentials:
   * **Google Gemini (PaLM) API**: Enter your Google AI API key.
   * **SMTP Account**: Enter your email provider credentials.
5. In the **HTTP Request** node, ensure `X-Api-Key` matches your WAHA configuration.
6. Toggle the workflow to **Active**.
