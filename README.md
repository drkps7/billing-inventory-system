# Billing & Inventory Management System

A full-featured billing, GST invoicing, and inventory management system built for an electrical goods retailer — designed to replace handwritten registers with a fast, accurate digital workflow.

**🌐 Live demo:** [prakashelectricals.pages.dev](https://prakashelectricals.pages.dev)

---

## Screenshots

| Dashboard | New Bill |
|---|---|
| ![Dashboard](https://raw.githubusercontent.com/drkps7/billing-inventory-system/main/dashboard.png) | ![Billing](https://raw.githubusercontent.com/drkps7/billing-inventory-system/main/billing.png) |

| Product Master | Outstanding |
|---|---|
| ![Products](https://raw.githubusercontent.com/drkps7/billing-inventory-system/main/productmaster.PNG) | ![Outstanding](https://raw.githubusercontent.com/drkps7/billing-inventory-system/main/outstanding.PNG) |

| Reports |
|---|
| ![Reports](https://raw.githubusercontent.com/drkps7/billing-inventory-system/main/report.PNG) |

---

## Features

- **GST Tax Invoicing** — Retail Invoice, Tax Invoice, Quotation with correct CGST/SGST breakdown per Indian GST law
- **Party Management** — Customer ledger, outstanding balance tracking, payment history
- **Inventory** — 2,000+ SKU product master, stock-in history, low stock alerts, barcode label generation
- **Billing** — Cart persistence across page refresh, draft auto-save, bill editing with full audit trail
- **Payments** — Split payment modes (Cash, UPI, Credit), partial payment tracking
- **Credit Notes** — Issue credit notes against existing bills
- **Reports** — Sales, stock movement, party-wise outstanding
- **Data Export** — CSV export of all data (Bills, Payments, Parties, Stock)
- **Auto-log** — Every bill auto-logged to Google Sheets via Apps Script webhook
- **Dark mode** — Full matte-black dark theme
- **Multi-user** — Owner / Staff roles, session management

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 (JSX via Babel CDN, single HTML file) |
| Database | Firebase Firestore |
| Auth | Firebase Authentication (Email/Password) |
| Hosting | Cloudflare Pages |
| Logging | Google Apps Script webhook → Google Sheets |

**No build step. No npm. No bundler.** The entire application is one `index.html` file.

---

## Setup

### 1. Firebase

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable **Firestore Database** (start in test mode)
3. Enable **Authentication → Email/Password**
4. Go to Project Settings → Your Apps → Add Web App
5. Copy the config object

### 2. Configure the app

Open `index.html` and replace the `firebaseConfig` object:

```javascript
firebase.initializeApp({
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
});
```

Also update the `COMPANY` constant near the top with your business details.

### 3. Firestore Security Rules

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 4. Deploy

Upload `index.html` to [Cloudflare Pages](https://pages.cloudflare.com) via direct upload.

### 5. First run

1. Open the app → you'll see the Setup screen
2. Create your owner account
3. Go to **Settings → Company** and fill in your business details
4. Import your product master via **Settings → Data & Import**

---

## Project Structure

```
index.html              # Entire application — React JSX, CSS, Firebase init
new_products.json       # Schema for product import (replace with your data)
stock_update.json       # Schema for stock update operations
```

---

## Key Design Decisions

- **Single file architecture** — Cloudflare Pages doesn't accept `.js` uploads directly; everything is compiled and inlined into one HTML file for deployment
- **Firestore as backend** — No server needed; all business logic runs client-side with Firestore as the database
- **Draft persistence** — Billing cart saved to localStorage on every change so a page refresh never loses work
- **Atomic stock updates** — `FieldValue.increment()` used for stock changes to prevent race conditions during concurrent billing
- **GST-exclusive pricing** — Entered price is always the taxable base; GST is added on top (standard for B2B electrical trade)

---

## License

MIT — free to use and adapt for your own business.

---

*Built with [Claude AI](https://claude.ai)*
