# 🧾 Project Raseed — AI-Powered Receipt & Expense Manager for Google Wallet

### 🚀 Overview
**Project Raseed** is an AI-powered personal finance assistant that helps users **digitize, organize, and analyze** their receipts using **Google Wallet** and **Google AI technologies**.  
It enables users to upload photos or videos of receipts, automatically extract key details, track expenses, and receive personalized spending insights — all accessible directly through **Google Wallet passes**.

---

## 🎯 Objectives

- Simplify receipt management by **digitizing and analyzing receipts** automatically.
- Integrate seamlessly with **Google Wallet** to store digital receipts as passes.
- Provide **AI-powered insights** on user spending, savings, and budgeting.
- Support **multilingual interactions** and local-language queries.
- Help users make smarter financial decisions with actionable suggestions.

---

## ✨ Core Features

### 1. Multimodal Receipt Ingestion
- Upload photos, videos, or live camera feeds of receipts.
- Use **Gemini’s multimodal AI** or fallback **OCR (Tesseract)** for text extraction.
- Automatically extract:
  - Store name, date, and time
  - List of items with prices and quantities
  - Taxes, discounts, total amount
- Store structured data in **MongoDB**.

---

### 2. Local Language Query Support
- Users can ask in any language:
  - “What did I spend on groceries last month?”
  - “Do I have enough detergent left?”
- Queries handled by **Vertex AI Agent Builder** (Google’s conversational agent).
- The assistant returns text or generates Wallet passes (e.g., a shopping list).

---

### 3. Spending Analysis & Insights
- Categorize purchases (groceries, travel, utilities, etc.).
- Show spending breakdowns over time.
- Detect recurring expenses and suggest savings.
- Create Wallet passes summarizing insights like:
  - “Your grocery spending increased 10% this month.”
  - “You can save ₹500 by switching to a different product.”

---

### 4. Wallet Integration
- Use **Google Wallet API** to:
  - Create and manage **custom passes** for receipts and insights.
  - Add receipts directly to the user’s Google Wallet.
  - Send **push notifications** for updates and savings alerts.
- Passes contain:
  - Front: summary (total, merchant)
  - Back: full receipt details (itemized)
  - Links back to the assistant for deeper insights

---

### 5. Shopping & Pantry Assistant
- Use stored receipts to track items bought.
- Allow user queries like:
  - “What can I cook with items I bought last 2 weeks?”
  - “Create a shopping list for next week.”
- Generate **shopping list passes** for Google Wallet.

---

### 6. Data & Privacy Control
- All receipts are stored securely in MongoDB with encryption.
- Users can delete their data or export it to **Google Sheets / CSV**.
- Optional on-device OCR for privacy (no upload).

---

## 🧱 Tech Stack

| Layer | Technology | Description |
|-------|-------------|-------------|
| **Frontend** | React (Vite) | Responsive UI for uploads, insights, and wallet link |
| **Backend** | Node.js + Express | API server to handle uploads, AI calls, and Wallet integration |
| **Database** | MongoDB | Stores user receipts, items, and spending summaries |
| **AI / ML** | Google Vertex AI + Gemini | Multimodal receipt understanding and conversational intelligence |
| **OCR (fallback)** | Tesseract.js / OpenCV | Free local OCR for text extraction |
| **Cloud / Hosting** | Google Cloud Run or Render | Host backend APIs |
| **Authentication** | Google Sign-In | Easy and secure user authentication |
| **API Integration** | Google Wallet API | Create & manage passes programmatically |
| **Storage** | Cloud Storage / Firebase | Store receipt images & thumbnails |

---

## ⚙️ System Architecture (Simplified)

1. **Frontend (React)**  
   - User uploads receipt image/video.  
   - Shows extracted data and “Add to Wallet” button.  
   - Lets users query in their local language.

2. **Backend (Node.js / Express)**  
   - Handles API requests from frontend.  
   - Sends media to Gemini (or Tesseract for fallback).  
   - Stores structured receipt data in MongoDB.  
   - Integrates with Google Wallet API to generate passes.

3. **AI Layer (Google Vertex AI)**  
   - Multimodal receipt understanding.  
   - Query answering and insights generation.  
   - Spending trend detection and savings suggestions.

4. **Database (MongoDB)**  
   - `users`: profile, language preference.  
   - `receipts`: itemized data, total, date, category.  
   - `insights`: monthly summaries, trends.

5. **Google Wallet Integration**  
   - Custom pass templates for:
     - Receipts
     - Monthly insights
     - Shopping lists  
   - Push updates when new insights are available.

---

## 🧩 Detailed Implementation Plan

### **Phase 1 — Setup & Core Backend**
- Create a Google Cloud project.
- Enable:
  - Vertex AI API
  - Google Wallet API
  - Cloud Storage API
- Initialize Node.js + Express backend.
- Connect MongoDB (use Mongoose).
- Implement routes:
  - `/upload` → for receipt upload
  - `/analyze` → for OCR / Gemini processing
  - `/wallet/create` → create and return Wallet pass link
- Add basic JWT or Google Sign-In authentication.

---

### **Phase 2 — Receipt Extraction**
- Implement two modes:
  1. **Primary:** Gemini multimodal → returns structured JSON (item list, totals).
  2. **Fallback:** Tesseract OCR + regex text parsing.
- Store extracted data in MongoDB.

---

### **Phase 3 — Spending Analytics**
- Categorize expenses automatically (use NLP or pre-defined merchant mappings).
- Create aggregation pipelines to:
  - Calculate monthly category totals.
  - Detect recurring payments.
  - Generate insights (spending up/down trends).

---

### **Phase 4 — Wallet Integration**
- Set up Google Wallet issuer account (demo mode first).
- Create **Generic Pass Class** and **Pass Object** using Node.js SDK.
- Pass data:
  ```json
  {
    "merchant": "Big Bazaar",
    "total": "₹1250",
    "date": "2025-10-06",
    "items": ["Milk", "Bread", "Butter"],
    "tax": "₹45"
  }
  ```
- Add link: "View full insights →" back to assistant.
- Implement push notifications via Wallet Messages API.

### **Phase 5 — Local Language Queries**

- Connect Vertex AI Agent Builder (Dialogflow-like setup).
- Train with sample intents:
“How much did I spend on food?”
“Create a shopping list.”
- Backend → forwards user queries → Vertex AI → returns structured reply.

### **Phase 6 — Frontend Development**
 React pages:
- Upload Page: Upload receipts (image/video).
- Dashboard: Spending chart, category filters.
- Insights Page: AI-generated savings tips.
- Wallet Integration: Add/Manage passes.

- Use libraries:
Axios (API calls)
Chart.js or Recharts (spending graphs)
Lucide-react (icons)
TailwindCSS (UI styling)

### **Phase 7 — Optimization & Extras**

- Add Google Sheets export for all receipts.
- Add offline support (PWA) for mobile use.
- Use Cloud Run / Render for backend deployment.
- Enable Firebase Analytics to monitor usage.

### **🧠 Example Workflow**

- User opens app → uploads photo of a grocery receipt.
- Backend sends it to Gemini → returns structured data (merchant, items, total).
- Data stored in MongoDB.
- System creates a Wallet pass → user adds it to Google Wallet.
- Later, user asks: “How much did I spend on groceries last month?”
- Agent analyzes data → replies → updates Wallet pass with insights.
- User receives notification in Wallet: “You spent ₹3,200 on groceries. Save 15% next month!”
