# ⚡ EB Tracker — MESCOM Gruha Jyoti Monitor

A Progressive Web App (PWA) to track monthly electricity consumption in real-time, built specifically for **MESCOM (Mangalore Electricity Supply Company)** consumers enrolled in Karnataka's **Gruha Jyoti subsidy scheme**.

> Built to solve a real problem — our bill randomly jumped from ₹980 to ₹2314 in one month. Turned out crossing 200 units forfeits the entire Gruha Jyoti subsidy. This app makes sure that never happens again.

---

## 📱 Demo

> _Add a screenshot or screen recording GIF here_

---

## 🔍 The Problem

Under Karnataka's **Gruha Jyoti scheme**, households consuming ≤ 200 units/month receive a subsidy (58 free units for our household). Crossing 200 units — even by 1 unit — means:

- Zero subsidy applied
- All units charged at full rate
- Bill jumps by ~₹1000+ instantly

There was no way to track running consumption between billing cycles (MESCOM reads meters monthly). This app fixes that.

---

## ✨ Features

### 📊 Tracker Tab
- Log meter readings anytime (Cum. kWh screen on your meter)
- Real-time unit consumption with color-coded zone indicator
  - 🟢 Safe (0–150 units)
  - 🟡 Caution (151–185 units)
  - 🔴 Danger (186–200 units)
  - 💀 Subsidy Lost (200+ units)
- Days remaining in billing cycle
- Daily average consumption
- Projected end-of-cycle total
- Bill estimate — with subsidy vs without subsidy (verified against actual MESCOM bills)
- Full reading history with per-entry delta

### 🔌 Appliances Tab
- All household appliances pre-loaded with **real BEE star rating data**
  - Panasonic 5★ Inverter AC → 484W avg (from 774.19 units/1600hrs label)
  - Whirlpool 3★ Frost Free Fridge → 29W avg (from 253 units/yr label)
  - LG 5★ Smart Inverter Washing Machine → 0.067 units/cycle (from 0.0103 kWh/kg/cycle label)
  - Atomberg BLDC Fans → 28W each (vs 75W standard fans)
  - Havells Air Cooler, Instant Geyser, Water Pump, Laptop, and more
- Daily unit budget ring (visual progress indicator)
- Special UPS/Inverter tracker — log power cuts by duration
- Always-on auto calculation (Fridge, WiFi Router)
- **Meter vs Appliance Cross-Check** — compare actual meter delta against app estimate to find hidden consumers

### 🔐 Security
- Firebase Realtime Database sync — shared across family devices
- Read-only for family members, write access restricted to admin only
- Hidden admin login — tap logo 5× to authenticate

### 📲 PWA
- Installable on Android home screen (Chrome → Add to Home Screen)
- Works like a native app
- Auto day reset for appliance logs

---

## 🧮 Bill Calculation

Formula verified against actual MESCOM bills (LT-1 Domestic, FY 2025-26):

```
Fixed Charges     = Sanctioned Load (kW) × ₹145
Energy Charges    = Units × ₹5.80
FPPCA             = Units × ₹0.38
P&G Surcharge     = Units × ₹0.36
Tax (9%)          = Energy Charges × 0.09
─────────────────────────────────────────
Gross Total       = Sum of above

Gruha Jyoti Subsidy (if units ≤ 200):
  Subsidy = Fixed + (Entitlement Units × all per-unit rates) + tax on that

Net Payable = Gross Total − Subsidy
```

Tariff source: **MESCOM Electricity Tariff 2026–2028, KERC Order dated 27th March 2025**

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML/CSS/JS (PWA) |
| Database | Firebase Realtime Database |
| Auth | Firebase Authentication (Email/Password) |
| Fonts | Syne + Space Mono (Google Fonts) |
| Hosting | GitHub Pages |

---

## 🚀 Setup

### 1. Clone the repo
```bash
git clone https://github.com/Sanketh2o11/eb-tracker.git
cd eb-tracker
```

### 2. Configure Firebase
Create a Firebase project and replace the config in `index.html`:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

> ⚠️ Never commit real API keys. Use environment variables in production.

### 3. Firebase Security Rules
```json
{
  "rules": {
    ".read": true,
    ".write": "auth != null && auth.token.email == 'your@email.com'"
  }
}
```

### 4. Enable Authentication
Firebase Console → Authentication → Sign-in method → Email/Password → Enable

### 5. Deploy
Push to GitHub and enable GitHub Pages on the `main` branch.

---

## 📋 How to Use

### Monthly Setup (when bill arrives ~11th of each month)
1. Open app → tap ⚙️
2. Update **Billing Start Reading** (Pres Rdg from new bill)
3. Update **Billing Start Date**
4. Tap **🔄 New Cycle** to clear history

### Daily Usage
1. Go to your meter → press button until **Cum. kWh** screen shows
2. Open app → enter reading → tap **LOG**
3. Check zone color — stay green!

### Admin Login (write access)
Tap the **⚡ EB Tracker** logo 5 times → enter password

---

## 📁 Project Structure

```
eb-tracker/
└── index.html      # Single-file PWA (HTML + CSS + JS)
└── README.md
```

> Refactor to modular file structure planned.

---

## 🗺 Roadmap

- [ ] Migrate to React with proper component structure
- [ ] Monthly history with Chart.js graphs
- [ ] Export bill summary as PDF
- [ ] Web Push notifications (alert at 150, 180, 200 units)
- [ ] Service Worker for true offline support
- [ ] Jest unit tests for billing calculator
- [ ] Environment variables for Firebase config

---

## 👤 Author

**Sanketh** — Final year AI/ML Engineering student, SMVITM Udupi  
GitHub: [@Sanketh2o11](https://github.com/Sanketh2o11)

---

## 📄 License

MIT License — feel free to adapt for your ESCOM (BESCOM, HESCOM, CESC, GESCOM).

---

## 🙏 Data Sources

- MESCOM Electricity Tariff 2026–2028 (KERC Order 27th March 2025)
- BEE Star Rating labels (Panasonic, Whirlpool, LG appliances)
- Karnataka Gruha Jyoti scheme guidelines
