# ♟️ Queen Vee Chess Initiative — Campaign Website

A high-fidelity, premium, responsive campaign and donation landing page for the **Queen Vee Chess Initiative**, bringing structured chess programmes, critical thinking, mentorship, and hope to children inside Internally Displaced Persons (IDP) camps.

Live features include an interactive theme engine, classical typography, custom iconography, and a secure **Double-Action post-payment verification flow** (Google Sheets Background AJAX Logging + WhatsApp Redirect Confirmation).

---

## 🌟 Key Features

*   🌓 **Light & Dark Theme Engine**: High-contrast, custom-tailored dark surface (`#10100e`) and card variables with system theme auto-detection and `localStorage` state persistence.
*   ✒️ **EB Garamond Serif Headings**: Classical institutional serif typography providing premium academic credibility and humanitarian brand prestige.
*   🏆 **Unified Gold Icon System**: Solid, custom-styled SVG icons that flow seamlessly in styling, size, and weight inside a dark contrasting mission strip.
*   ⚡ **Dynamic "I've Paid" Triggers**: Button reveal animations inside donation cards only when a donor successfully copies local or international account details.
*   🪟 **Glassmorphic Verification Modal**: Glassmorphism backdrop blur modal collecting individual/organization donor names, optional contact info, anonymous status, and currency-adaptive amounts (`₦` vs `$`).
*   📝 **Double-Action Verification Flow**:
    1.  **Background Google Sheets AJAX**: Secure, preflight-compliant background `fetch()` logging directly to your spreadsheet web app in `no-cors` mode.
    2.  **Interactive WhatsApp Confirmation**: Immediate receipt compiler generating a clean confirmation message and populating it to a target WhatsApp link (**+234 80 6574 8340**).

---

## 📂 Repository Structure

When you upload this repository to **GitHub** and connect it to **Vercel** or **Netlify**, the hosting providers will instantly detect and deploy the site from the root directory:

```
DONATE/                     <-- Root Repository Folder
├── index.html              <-- Main static campaign website
├── README.md               <-- Professional repository documentation (This file)
├── .gitignore              <-- Standard Git file exclusions
└── images/                 <-- High-quality optimized image directory
    ├── logo.png            <-- Official Shield emblem logo
    ├── support_icon.png    <-- Hand-holding-heart white-inverted button icon
    ├── chess_pieces_icon.png <-- Solid, CSS-filtered mission strip icon
    ├── slide1.jpg          <-- Interactive IDP campaign photo 1
    ├── slide2.jpg          <-- Interactive IDP campaign photo 2
    ├── slide3.jpg          <-- Interactive IDP campaign photo 3
    ├── slide4.jpg          <-- Interactive IDP campaign photo 4
    ├── zenith_logo.png     <-- High-contrast inline bank logo
    └── uba_logo.png        <-- United Bank for Africa logo
```

---

## ⚡ Deployment & Google Sheets Setup

To make your verification form write directly to a **Google Sheet**:

### 1. Set Up Google Sheet Script
1.  Create a new Google Sheet (e.g., `Queen Vee Donation Verification Logs`).
2.  Open **Extensions > Apps Script** in the menu bar.
3.  Delete any default script, paste the following 15-line code, and click Save:
    ```javascript
    function doPost(e) {
      var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
      try {
        var data = JSON.parse(e.postData.contents);
        sheet.appendRow([
          new Date(),
          data.referenceId || "",
          data.donor || "",
          data.anonymous ? "Yes" : "No",
          data.type || "",
          data.email || "",
          data.phone || "",
          data.destination || "",
          data.amount || ""
        ]);
        return ContentService.createTextOutput(JSON.stringify({ status: "success" }))
          .setMimeType(ContentService.MimeType.JSON);
      } catch (err) {
        return ContentService.createTextOutput(JSON.stringify({ status: "error", message: err.message }))
          .setMimeType(ContentService.MimeType.JSON);
      }
    }
    ```

### 2. Deploy Web App
1.  Click **Deploy > New Deployment** (top right).
2.  Select **Web app** as the deployment type (via the gear icon).
3.  Set "Execute as" to **Me (your-email@gmail.com)**.
4.  Set "Who has access" to **Anyone** (this is critical for allowing public form requests).
5.  Click **Deploy**, authorize permissions, and copy the generated **Web App URL** (ends with `/exec`).

### 3. Insert Web App URL
Your Web App URL is already configured inside `index.html` on the global variable block:
```javascript
const GOOGLE_SHEET_URL = 'https://script.google.com/macros/s/AKfycbyrl5C7qYviu6dsxVhQh3sXm0jd5AbzdZzfw1ZTSKp1_vAcKc4_LlNb5PAjF83hgYh39w/exec';
const WHATSAPP_PHONE = '2348065748340'; // Your team's WhatsApp contact line
```

---

## 🚀 One-Click Hosting (Vercel & Netlify)

1.  Create a free account on [Vercel](https://vercel.com) or [Netlify](https://netlify.com).
2.  Select **Import Project / New Site from Git** and choose your uploaded GitHub repository.
3.  Vercel/Netlify will automatically detect `index.html` at the root and host it on a premium global CDN instantly!
