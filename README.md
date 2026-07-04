# Prakash Electricals — Billing & Inventory System

A full-featured billing, invoicing, and inventory management system built for an electrical goods retailer.

**Live demo:** [prakashelectricals.pages.dev](https://prakashelectricals.pages.dev)

---

## Features

- **GST Tax Invoicing** — Retail Invoice, Tax Invoice, Quotation with correct CGST/SGST breakdown
- **Party Management** — Customer ledger, credit limits, outstanding balance tracking
- **Inventory** — 2,000+ SKU product master, stock-in history, low stock alerts, barcode labels
- **Billing** — Cart persistence across refresh, draft auto-save, bill editing with supersede trail
- **Payments** — Split payment modes (Cash, UPI, Credit), partial payment tracking
- **Credit Notes** — Issue credit notes against existing bills
- **Reports** — Sales, stock, party-wise outstanding
- **Export** — CSV export of all data (Bills, Payments, Parties, Stock)
- **Auto-log** — Every bill auto-logged to Google Sheets via Apps Script webhook
- **Dark mode** — Full matte-black dark theme
- **Multi-user** — Owner / Staff roles, session management

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 (JSX via Babel CDN, single-file) |
| Database | Firebase Firestore |
| Auth | Firebase Authentication |
| Hosting | Cloudflare Pages |
| Logging | Google Apps Script webhook |

No build step. No npm. One `index.html` file.

---

## Setup

### 1. Firebase

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable **Firestore Database** (start in test mode)
3. Enable **Authentication → Email/Password**
4. Go to Project Settings → Your Apps → Add Web App
5. Copy the config object

### 2. Configure the app

Open `src/index.html` and replace the `firebaseConfig` object:

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

Also update the `COMPANY` constant with your business details.

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

Upload `src/index.html` to [Cloudflare Pages](https://pages.cloudflare.com) via direct upload (zip).

### 5. First run

1. Open the app → you'll see the Setup screen
2. Create your owner account
3. Go to Settings → Company and fill in your details
4. Go to Settings → Data & Import to load your product master

---

## Screenshots

| Dashboard | New Bill | Product Master |
|---|---|---|
| *(add screenshot)* | *(add screenshot)* | *(add screenshot)* |

---

## Project Structure

```
src/
└── index.html          # Entire app — React JSX, CSS, Firebase init
sample-data/
├── new_products.json   # Schema for product import
└── stock_update.json   # Schema for stock update operations
```

---

## License

MIT — free to use and adapt for your own business.

---

*Built with [Claude](https://claude.ai)*
