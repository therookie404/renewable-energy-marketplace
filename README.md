# Lumina — India's P2P Renewable Energy Marketplace

> Trade clean solar, wind & biogas energy peer-to-peer. No DISCOM middlemen. Real CO₂ tracking.

[![Made in India](https://img.shields.io/badge/Made%20in-India%20🇮🇳-orange)](https://github.com)
[![Firebase](https://img.shields.io/badge/Backend-Firebase-FFCA28?logo=firebase)](https://firebase.google.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

##  Live Demo

Open `index.html` directly in any browser — no build step, no server required.

**Quick Demo Logins** (available on the Login page):
| Role | Email | Password |
|------|-------|----------|
| Producer | producer@demo.com | 123456 |
| Consumer | consumer21@demo.com | 123456 |
| Investor | investor@demo.com | 123456 |

---

##  File Structure

```
lumina/
├── index.html          # Entire application — single self-contained file
├── README.md           # This file
└── LICENSE             # MIT License
```

> **Single-file architecture** — all HTML, CSS, and JavaScript live in `index.html`. Firebase is loaded via CDN script tags. No npm, no build tools, no dependencies to install.

---

## Features

### 3 User Roles
- ** Producer** — List solar/biogas/wind energy, manage listings, view earnings analytics
- **Consumer** — Browse marketplace, buy energy, track CO₂ savings, download carbon certificate
- **Investor** — Browse projects, use ROI calculator, express funding interest

### Core Platform
- Firebase Authentication (email/password)
- Firestore real-time database
- Verified producer badge system (4.5★ / 3+ reviews)
- Mock payment checkout with card formatting
- Toast notifications + skeleton loaders

### Standout Features
-  **CO₂ Certificate** — printable carbon credit certificate after every purchase
-  **AI Price Suggestion** — market-aware price hints for producers
-  **AI Price Prediction Chart** — forecasted energy price trends by type
-  **India Energy Heatmap** — live energy availability by state
-  **Savings Calculator** — shows how much a consumer saves vs grid rates
-  **Community Live Feed** — real-time energy trade activity stream
-  **Notification Bell** — cross-role alerts (e.g. investor interest → producer)
-  **Light / Dark Mode** — theme toggle with localStorage persistence
-  **Confetti** — celebration animation on successful purchase

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML5, CSS3, JavaScript (ES2020) |
| Auth | Firebase Authentication (compat SDK v9.23) |
| Database | Firebase Firestore (compat SDK v9.23) |
| Charts | Chart.js 4.4.0 |
| Icons | Font Awesome 6.5 |
| Fonts | Plus Jakarta Sans, DM Sans (Google Fonts) |
| Hosting | Any static host (GitHub Pages, Firebase Hosting, Netlify) |

---


##  Regulatory Context

Lumina's P2P model is aligned with:
- **CERC P2P Energy Trading Regulations 2023** (Central Electricity Regulatory Commission)
- **India National Smart Metering Programme** (NSMP) — 250M meters by 2026
- Active pilot programs in Maharashtra, Gujarat & Andhra Pradesh

---

## 📜 License

MIT © 2025 Lumina Team
