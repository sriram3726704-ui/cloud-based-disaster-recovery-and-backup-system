# CloudVault – Cloud-Based Disaster Recovery and Backup System

![Project Type](https://img.shields.io/badge/Project-College%20Final%20Year%20%2F%20Capstone-blue.svg)
![Technology](https://img.shields.io/badge/Stack-HTML5%20%7C%20CSS3%20%7C%20JavaScript-orange.svg)
![Cloud](https://img.shields.io/badge/Backend-Firebase%20Auth%20%7C%20Storage%20%7C%20Firestore-yellow.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

---

## 📌 Project Overview

**CloudVault** is an enterprise-grade, cloud-based disaster recovery and automated backup system. Designed for college project demonstrations and capstone evaluations, it enables users to protect critical documents (PDF, DOCX, JPG, PNG, TXT, ZIP, CSV, etc.) from hardware failure, accidental deletion, and ransomware.

The project demonstrates core concepts in cloud computing and data resilience:
- **Zero Data Loss Principle (RPO = 0)**
- **Sub-5-second Recovery Time Objective (RTO < 5s)**
- **The 3-2-1 Industry Backup Standard**
- **Strict Multi-Tenant User Isolation & Server-Side Security Rules**

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend UI** | HTML5 Semantic Structure | Clean, accessible dashboard and document explorer |
| **Styling** | Vanilla CSS3 (Custom Design System) | Modern enterprise blue & slate aesthetic, responsive layout, glassmorphism |
| **Logic & State** | Vanilla JavaScript (ES6 Modules) | Modular client-side architecture without heavyweight framework dependencies |
| **Authentication** | Firebase Authentication | Secure Email/Password registration, session management, and password recovery |
| **Cloud Storage** | Firebase Cloud Storage | High-durability object storage for replicated user files with progress streaming |
| **Metadata & Logs** | Google Cloud Firestore | NoSQL document database storing backup metadata and tamper-proof audit trails |
| **Hosting** | Firebase Hosting | Production-grade CDN deployment with instant global replication |

---

## 📂 Project Structure

```
cloud-based-disaster-recovery-and-backup-system/
├── index.html                   # Main application entry point & dashboard UI
├── css/
│   ├── main.css                 # Design tokens, typography, buttons, alert banners
│   ├── dashboard.css            # Sidebar, top header, metric cards, category distribution
│   ├── components.css           # Dropzone, tables, modals, toast notifications, badges
│   └── responsive.css           # Mobile, tablet, and desktop media queries
├── js/
│   ├── firebase-config.js       # Active Firebase configuration & service initializer
│   ├── firebase-config.template.js # Template with placeholders & configuration instructions
│   ├── auth.js                  # Authentication, profile management, and session state
│   ├── storage-service.js       # Firebase Storage file upload, progress tracking & delete
│   ├── firestore-service.js     # Firestore metadata persistence, queries & audit logging
│   ├── dr-service.js            # Disaster recovery metrics (RPO/RTO) & simulation drill
│   ├── ui-manager.js            # Formatting, modal handlers, toasts & tab switching
│   └── app.js                   # Application controller and event wiring
├── firestore.rules              # Server-side Firestore security rules (enforcing UID isolation)
├── storage.rules                # Server-side Cloud Storage security rules (path-level isolation)
├── firebase.json                # Firebase Hosting and security deployment configuration
├── .firebaserc                  # Firebase project identifier mapping
├── package.json                 # Project scripts (local server, deployment)
├── VIVA_QUESTIONS.md            # Comprehensive viva preparation guide for college faculty
└── README.md                    # Setup and documentation guide
```

---

## 🚀 Step-by-Step Setup Instructions

### Step 1: Create a Firebase Project
1. Navigate to the [Firebase Console](https://console.firebase.google.com/).
2. Click **"Add Project"**, name it (e.g., `cloudvault-dr-system`), and click **Continue**.
3. (Optional) Disable Google Analytics for simplicity, then click **Create Project**.

### Step 2: Enable Firebase Authentication
1. In the Firebase sidebar, click **Build** &rarr; **Authentication**.
2. Click **Get Started**, choose the **Email/Password** provider, and toggle **Enable**.
3. Click **Save**.

### Step 3: Enable Cloud Firestore
1. In the sidebar, click **Build** &rarr; **Firestore Database**.
2. Click **Create Database**. Choose your nearest location (e.g., `nam5 (us-central)` or `asia-south1`).
3. Select **Start in test mode** for development, then click **Create**.
4. To enforce production security rules, deploy the provided `firestore.rules` file (see Step 6).

### Step 4: Enable Cloud Storage
1. In the sidebar, click **Build** &rarr; **Storage**.
2. Click **Get Started**, select **Start in test mode**, and choose a storage bucket region.
3. Click **Done**.

### Step 5: Register Web App & Obtain Configuration
1. In the Firebase Console, click the **Gear Icon (Project Settings)** &rarr; **General**.
2. Scroll to **Your apps**, click the **Web icon (`</>`)**, and register the app (e.g., `CloudVault Web`).
3. Copy the `firebaseConfig` object shown on the screen:
   ```javascript
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "cloudvault-dr.firebaseapp.com",
     projectId: "cloudvault-dr",
     storageBucket: "cloudvault-dr.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
4. Open [js/firebase-config.js](file:///c:/Users/Sriram/OneDrive/Desktop/cloud%20based%20disaster%20recovery%20and%20backup%20system/js/firebase-config.js) in your editor and replace the placeholder credentials with your actual project keys.

> **💡 Faculty / Viva Quick Testing Feature:**  
> You can also launch the app directly! If Firebase keys are not yet configured in the file, CloudVault displays a **Setup Wizard** button where you can paste the JSON config directly into the browser, OR click **"Explore Demo Preview"** to run simulated demonstrations.

### Step 6: Deploy Security Rules
To ensure strict user data privacy, deploy the security rules:
```bash
# Install Firebase CLI if not already installed
npm install -g firebase-tools

# Login to Firebase
firebase login

# Deploy rules
firebase deploy --only firestore:rules,storage
```

---

## 💻 Running the Application Locally

You can run CloudVault using any simple static web server:

### Option A: Using Node.js (Recommended)
```bash
npm start
```
*Opens at `http://localhost:3000`*

### Option B: Using Python
```bash
python -m http.server 3000
```
*Opens at `http://localhost:3000`*

### Option C: Using VS Code Live Server
Right-click on `index.html` and select **"Open with Live Server"**.

---

## 🌐 Deploying to Firebase Hosting

To host CloudVault live on the internet:
```bash
# Initialize Firebase Hosting in the project directory
firebase init hosting

# When prompted:
# 1. Use an existing project -> Select your Firebase project
# 2. What do you want to use as your public directory? -> . (current directory)
# 3. Configure as a single-page app? -> Yes
# 4. Set up automatic builds with GitHub? -> No
# 5. Overwrite index.html? -> No

# Deploy to live global CDN
firebase deploy --only hosting
```
Your live URL will be: `https://your-project-id.web.app`

---

## 🛡️ Disaster Recovery Concepts Explained

### 1. Recovery Point Objective (RPO)
The maximum allowable age of files recovered from backup storage after a disaster.
- **Traditional Backup:** 24 hours (Daily overnight tape backups).
- **CloudVault Standard:** < 1 hour via continuous cloud synchronization.

### 2. Recovery Time Objective (RTO)
The maximum acceptable duration of time that can elapse before data is restored.
- **Traditional Retrieval:** 4 to 48 hours for physical retrieval.
- **CloudVault Standard:** < 5 seconds via high-throughput cloud streaming.

### 3. The 3-2-1 Backup Strategy
- **3 Copies:** 1 primary working copy + 2 backup replicas.
- **2 Media Types:** Local storage + remote cloud object storage.
- **1 Off-site:** Hosted across geographically redundant Google Cloud data centers.

### 4. Why Can't Deleted Files Be Restored?
When a user explicitly deletes a file, CloudVault calls `deleteObject()` on Firebase Storage to prevent unnecessary cloud storage billing and comply with data privacy regulations (GDPR "Right to be Forgotten"). Deleted files cannot be restored unless an independent external snapshot or bucket versioning policy is preserved.

---

## 🎓 College Project Demonstration Script

When presenting CloudVault to your college faculty or examiner:
1. **Show Login / Multi-Tenant Isolation:** Sign in with two different accounts to prove User A cannot view User B's backups.
2. **Demonstrate Live File Upload:** Upload a PDF or Image. Highlight the live percentage progress bar, size validation (max 25MB), and MIME validation.
3. **Show Cloud Storage & Firestore Sync:** Show the Firebase Console side-by-side to verify the file appears in Cloud Storage under `users/{uid}/files/` and its metadata appears in Firestore.
4. **Trigger a File Restore / Download:** Click "Restore" to download the file directly from the cloud. Show the activity audit log updating with a timestamped `RESTORE` event.
5. **Run the Disaster Recovery Simulation Drill:** Click "Simulate DR Drill" on the Disaster Recovery Hub tab. Explain the 5 stages of failover, RTO measurement, and hash integrity checks.
6. **Explain Security Rules:** Show `firestore.rules` and `storage.rules` to explain how access is restricted server-side via `request.auth.uid`.
