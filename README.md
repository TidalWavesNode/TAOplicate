<p align="center">
  <img width="482" height="482" alt="taoplicate-top-left" src="https://github.com/user-attachments/assets/b5a6e44a-8079-4246-a383-29849c3f5044" />
</p>

# TAOplicate: A Real-Time BitTensor Copy-Trading Bot 

Real-time, PM2-managed bot that mirrors **stake/unstake** actions from chosen BitTensor **hotkeys** across **all subnets** using your wallet — with Discord alerts, daily summaries (00:00 UTC), automatic low-balance pause/resume, **SQLite analytics**, **weighted proportional mode**, and **dry-run** simulation.

---

![status](https://img.shields.io/badge/status-alpha-blue)
![python](https://img.shields.io/badge/python-3.10%2B-blue)
![license](https://img.shields.io/badge/license-MIT-green)
![bittensor](https://img.shields.io/badge/bittensor-finney-orange)

---

## ✨ Features
- **Global copy-staking** — watches your **hotkeys** on **every subnet**; mirrors stake adds/removes.
- **Realtime** via **local Subtensor node WS** (primary), **Finney WS** fallback, plus **polling** backup.
- **Trade sizing modes** — `fixed`, `proportional`, or **weighted proportional** (per-hotkey weight).
- **Discord**:
  - **Live alerts** (color embeds on each mirrored trade).
  - **Daily summary** at **00:00 UTC** (totals, net gain/loss, wallet balance with **📈/📉 trend**).
- **Safety** — auto-pause when wallet TAO `< low_balance`, auto-resume when `>= resume_balance`.
- **Analytics** — every action written to **SQLite** (`~/.taoplicate/taoplicate.db`).
- **Ops-friendly** — flags for `--dry-run` and `--summary-now`, plus PM2 integration.

---

## 🧱 Architecture

```
<img width="600" height="919" alt="image" src="https://github.com/user-attachments/assets/360cb95e-eb1b-467b-8e35-601a6b92ed22" />
<img width="600" height="919" alt="image" src="https://github.com/user-attachments/assets/360cb95e-eb1b-467b-8e35-601a6b92ed22" />

```

## Install
Prereqs: Python 3.10+, Node.js (for PM2), btcli configured, and (recommended) a local subtensor node exposing WebSocket ws://127.0.0.1:9944

### Clone and enter
```
git clone https://github.com/TidalWavesNode/TAOplicate.git
```

```
cd TAOplicate
```

### Python deps
```
python3 -m venv .venv && source .venv/bin/activate
```

```
pip install -r requirements.txt
```

### (Optional) Install PM2 for supervision
```
npm i -g pm2
```

## 🚀 First-Run Setup
```
python3 taoplicate.py setup
```

Prompts include:
```
Network (e.g., finney)
Your wallet name (--wallet-name)
Fixed TAO per trade (only used if fixed mode)
Watched hotkeys (each line supports optional weight, e.g. 5Eabc.. 0.6)
Polling seconds (fallback heartbeat; 20–60s typical)
Trade type: fixed or proportional
Discord webhooks: one for live alerts, one for daily summary
Low/resume balance thresholds (auto-pause/resume)
```

Files created:
```
~/.taoplicate/taoplicate_config.json
~/.taoplicate/taoplicate_state.json
~/.taoplicate/taoplicate.db
~/.taoplicate/taoplicate.log
~/.taoplicate/last_balance.json
```
## Run
```
python3 taoplicate.py run --summary-now
```
### add --dry-run to simulate without btcli transactions

### PM2:
```
pm2 start ecosystem.config.js --name bt-taoplicate -- "run --summary-now"
```
```
pm2 logs bt-taoplicate --lines 200
```
```
pm2 save
```

## 🖼️ Discord Examples

```Live trade embed
“Stake Added” (green) or “Stake Removed” (red) with subnet, hotkey, Δ, mirrored amount.
Daily summary (00:00 UTC, neutral color)
Total trades, subnets touched, total staked/unstaked
🟩/🟥 Net gain/loss
💰 Wallet balance with 📈/📉 since last report
```

## 🛡️ Safety & Ops
```
Auto-pause when balance < low_balance → Discord alert
Auto-resume when balance >= resume_balance → Discord notice
Event-driven via WS, with Finney fallback and polling safety net
Dry-run for rehearsals
SQLite for audit/analytics:
`sqlite3 ~/.taoplicate/taoplicate.db \
  "SELECT timestamp,action,netuid,hotkey,amount,delta FROM trades ORDER BY id DESC LIMIT 20;"`
```
## 🧪 Tips
If you have many watched hotkeys, prefer event mode (local node) and set polling 60–120s.
Use weights to bias toward trusted wallets.
Add min/max caps in code if you want to clamp mirrored amounts.

## 🤝 Contributing
PRs welcome. Ideas: top subnets in summary, Flask dashboard, Telegram bot, auto-weighting by performance.

# 📌 Disclaimer
This bot is educational and experimental. It does not constitute financial advice. Use at your own risk.

---

### `LICENSE`
```
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
