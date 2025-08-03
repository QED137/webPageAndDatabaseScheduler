# 🧠 CineBot Scheduler – Kamatera + Neo4j Aura

This repository automates the **daily start and stop schedule** for my [CineBot RAG App](https://cinebot.janmajay.de/), hosted on **Kamatera VPS** and connected to a **Neo4j AuraDB**.

---

## ⚙️ What It Does

- 🚀 Starts the Kamatera VPS **daily at 08:00 (Germany time)**
- 🛑 Stops the VPS **daily at 20:00 (Germany time)**
- 💸 Reduces cloud costs by running the server only during active hours
- 🤖 Uses **GitHub Actions** + **Kamatera API**

---

## 🔗 Infrastructure Overview

| Component        | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| 🔌 **App**        | [cinebot.janmajay.de](https://cinebot.janmajay.de/) – Flask-based RAG app with OpenAI |
| ☁️ **Server**     | Kamatera VPS (1 vCPU, 2GB RAM, 50GB SSD)                                    |
| 🧠 **Database**   | Neo4j AuraDB (Free Tier – sleeps when idle)                                 |
| ⏱️ **Scheduler**  | GitHub Actions (`.github/workflows/kamatera-scheduler.yml`)                 |

---

## 🛠️ How It Works

1. GitHub Actions runs on a daily cron schedule:
   - `06:00 UTC` → Starts the VPS (08:00 Germany time)
   - `18:00 UTC` → Stops the VPS (20:00 Germany time)
2. Authenticates with Kamatera using secure API credentials.
3. Sends start/stop requests using Kamatera API endpoints.

---

## 🔐 Setup Instructions

1. Fork this repo or add the workflow to your project.
2. Create the following GitHub **Secrets**:
   - `KAMATERA_API_ID`
   - `KAMATERA_API_SECRET`
   - `KAMATERA_SERVER_ID`
3. GitHub Actions will handle the rest automatically.

---

## 🧾 Why This Setup?

- **Neo4j AuraDB** auto-sleeps when unused — ideal for cost-saving.
- **Kamatera** charges hourly — great for scheduled uptime.
- This setup ensures:
  - ✅ App runs during working hours
  - 💤 Neo4j wakes up naturally when needed
  - 🔐 VPS stays off after hours to reduce cost

---

## 📅 Customize Schedule

Update the CRON times in `.github/workflows/wake-ragapp.yml` if you want a different schedule.

```yaml
# Default: Start at 6:00 UTC (8 AM Germany), Stop at 18:00 UTC (8 PM Germany)
- cron: '0 6 * * *'   # Start
- cron: '0 18 * * *'  # Stop
