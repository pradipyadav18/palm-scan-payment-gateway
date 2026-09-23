# 🦾 Secure Biometric Payment System

### Hand Geometry + CNN Fusion | Risk-Based Multi-Factor Authentication

A full-stack biometric payment system that combines **hand geometry analysis** and **deep-learning-based palm feature extraction** to authenticate users before processing financial transactions.

The system uses a **risk-based authentication model**, where additional authentication factors are required as the transaction amount increases.

---

## 🚀 Project Overview

The **Palm Scan Payment Gateway** is designed to provide biometric authentication for digital payments using a hybrid approach:

* **Hand Geometry Analysis** using MediaPipe
* **CNN-based Feature Extraction** using MobileNetV2
* **Risk-based Multi-Factor Authentication**
* **Razorpay Payment Gateway**
* **Email OTP Verification**
* **Encrypted Biometric Templates**
* **User and Admin Dashboards**
* **Transaction Monitoring and Audit Logs**

The application consists of a **React frontend** and an **asynchronous FastAPI backend**, with MongoDB used for persistent data storage.

---

## ✨ Key Features

### 🔐 1. Hybrid Biometric Authentication

The system combines two types of biometric features:

#### Hand Geometry

MediaPipe is used to detect hand landmarks and generate geometric features such as:

* Finger lengths
* Finger thickness
* Joint relationships
* Finger ratios
* Hand landmark positions

The implementation uses a **51-point feature representation** for geometric comparison.

#### CNN Feature Extraction

A **MobileNetV2-based CNN** is used to extract high-dimensional visual features from the palm image.

The extracted features capture visual and spatial information that complements the geometric representation.

### Hybrid Matching

The biometric verification process combines:

```text
Palm Image
     │
     ├──────────────► MediaPipe
     │                    │
     │                    ▼
     │             Hand Geometry
     │
     └──────────────► MobileNetV2
                          │
                          ▼
                   CNN Embedding
                          │
                          ▼
                  Hybrid Matching
                          │
                          ▼
                  Similarity Score
```

The implemented matching threshold is configured around **95% similarity**.

---

# 🔑 2. Risk-Based Authentication

The system dynamically increases authentication requirements based on transaction value.

| Transaction Amount | Authentication             |
| ------------------ | -------------------------- |
| `< ₹2,000`         | Palm Biometric             |
| `₹2,000 - ₹10,000` | Palm Biometric + PIN       |
| `> ₹10,000`        | Palm Biometric + Email OTP |

### Authentication Flow

```text
                Payment Request
                       │
                       ▼
                Check Amount
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      < ₹2,000    ₹2,000-₹10,000   > ₹10,000
          │            │            │
          ▼            ▼            ▼
      Biometric    Biometric      Biometric
                       +              +
                      PIN            OTP
          │            │            │
          └────────────┼────────────┘
                       ▼
                Payment Processing
```

This approach allows the application to apply different authentication requirements depending on transaction risk.

---

# 💳 3. Payment Integration

The system integrates with **Razorpay** for payment processing.

The payment flow is:

```text
User
 │
 ▼
Select Payment
 │
 ▼
Palm Verification
 │
 ▼
Risk Assessment
 │
 ├── Low Amount ──────────► Continue
 │
 ├── Medium Amount ───────► PIN Verification
 │
 └── High Amount ─────────► Email OTP
                              │
                              ▼
                       Authentication
                              │
                              ▼
                       Razorpay Payment
```

Razorpay credentials should be configured using environment variables and should not be committed to the repository.

---

# 📧 4. Email OTP Authentication

For high-value transactions, the system generates a temporary OTP and sends it to the user's registered email address through SMTP.

```text
High-Value Transaction
          │
          ▼
    Biometric Match
          │
          ▼
      Generate OTP
          │
          ▼
      Email via SMTP
          │
          ▼
      User Enters OTP
          │
          ▼
    Verify OTP
          │
          ▼
   Continue Payment
```

---

# 🛡️ 5. Biometric Data Protection

Biometric information is sensitive, so the application uses encryption before storing biometric templates.

The project uses:

**AES-256 encryption**

for protecting stored biometric signatures/templates.

Sensitive information such as:

* Bank account numbers
* Email addresses
* Biometric information

is handled with additional protection or masking where applicable.

---

# 👤 6. User Dashboard

The user dashboard provides functionality such as:

* Biometric registration
* Biometric verification
* Payment initiation
* Transaction history
* Registered payment nodes
* Masked sensitive information
* Biometric verification status

The interface includes a modern biometric/HUD-style design.

---

# 👨‍💼 7. Admin Dashboard

The admin dashboard provides security and transaction monitoring capabilities.

It includes:

* Transaction audit logs
* Biometric match scores
* Authentication results
* Transaction status
* Security-related monitoring
* User activity information

---

# 🛠️ Technology Stack

## Frontend

* React.js
* JavaScript
* Tailwind CSS
* Framer Motion
* Lucide Icons

## Backend

* Python
* FastAPI
* Uvicorn
* Async API architecture

## Machine Learning / Computer Vision

* MediaPipe
* PyTorch
* MobileNetV2
* CNN Feature Extraction

## Database

* MongoDB
* MongoDB Compass

## Payment & Communication

* Razorpay
* Gmail SMTP

## Development Tools

* Git
* GitHub
* VS Code / IntelliJ
* Postman

---

# 📂 Project Structure

```text
palm-scan-payment-gateway/
│
├── backend/
│   │
│   ├── app/
│   │   │
│   │   ├── biometric/
│   │   │   ├── matcher.py
│   │   │   └── hand_detector.py
│   │   │
│   │   ├── payment/
│   │   │   └── ...
│   │   │
│   │   ├── admin/
│   │   │   └── ...
│   │   │
│   │   └── main.py
│   │
│   ├── requirements.txt
│   └── ...
│
├── frontend/
│   │
│   ├── src/
│   │   │
│   │   ├── components/
│   │   │   └── PaymentModal.jsx
│   │   │
│   │   ├── pages/
│   │   │   └── Dashboard.jsx
│   │   │
│   │   └── services/
│   │       └── api.js
│   │
│   ├── package.json
│   └── ...
│
├── seed_admin.py
├── .env.example
├── .gitignore
├── package-lock.json
└── README.md
```

---

# ⚙️ Installation & Setup

## 1. Prerequisites

Make sure the following are installed:

* Python 3.10+
* Node.js 18+
* npm
* MongoDB Community Server
* MongoDB Compass
* Git

---

# 🗄️ 2. MongoDB Setup

Install MongoDB Community Server and MongoDB Compass.

Start MongoDB and connect using:

```text
mongodb://localhost:27017
```

Create/use the database:

```text
hand_biometrics_db
```

The application can use:

```text
mongodb://localhost:27017/hand_biometrics_db
```

---

# 🔧 3. Environment Configuration

Create a `.env` file based on `.env.example`.

Example:

```env
MONGODB_URL=mongodb://localhost:27017/hand_biometrics_db

SECRET_KEY=your_super_secret_key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440

RAZORPAY_KEY_ID=your_razorpay_key
RAZORPAY_KEY_SECRET=your_razorpay_secret

EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
```

### ⚠️ Security

Never commit the real `.env` file to GitHub.

Use:

```text
.env.example
```

to provide the required environment variable names without exposing secrets.

---

# 🐍 4. Backend Setup

Navigate to the backend directory:

```bash
cd backend
```

Create a virtual environment:

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# 👨‍💼 5. Seed Admin User

Run:

```bash
python seed_admin.py
```

This initializes the admin account according to the project's seed configuration.

---

# 🚀 6. Start Backend

From the appropriate project directory, start FastAPI using Uvicorn:

```bash
python -m uvicorn backend.app.main:app --port 8000 --reload
```

Backend will be available at:

```text
http://localhost:8000
```

FastAPI documentation:

```text
http://localhost:8000/docs
```

---

# ⚛️ 7. Frontend Setup

Open another terminal:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The frontend URL will be displayed by Vite in the terminal.

---

# 🔄 Complete System Flow

```text
                   ┌──────────────────┐
                   │      User        │
                   └────────┬─────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │ React Frontend   │
                   └────────┬─────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │  FastAPI Backend │
                   └────────┬─────────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
        ┌─────────────────┐   ┌─────────────────┐
        │ Biometric       │   │ Payment Module  │
        │ Verification    │   │                 │
        └────────┬────────┘   └────────┬────────┘
                 │                     │
        ┌────────┴────────┐            │
        ▼                 ▼            ▼
   MediaPipe          MobileNetV2   Razorpay
        │                 │
        └────────┬────────┘
                 ▼
          Hybrid Matching
                 │
                 ▼
          Risk Assessment
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
      Bio      Bio+PIN  Bio+OTP
        │        │        │
        └────────┼────────┘
                 ▼
           Payment Result
                 │
                 ▼
              MongoDB
```

---

# 🧠 How Biometric Matching Works

The biometric verification pipeline can be summarized as:

```text
Palm Image
    │
    ▼
Image Processing
    │
    ├───────────────┐
    ▼               ▼
MediaPipe       MobileNetV2
    │               │
    ▼               ▼
51-Point        CNN Embedding
Geometry            │
    │               │
    └───────┬───────┘
            ▼
      Feature Fusion
            │
            ▼
     Similarity Score
            │
            ▼
     Threshold Check
            │
       ┌────┴────┐
       ▼         ▼
    Match      Reject
```

The hybrid approach combines structural hand information with image-derived features rather than relying on only one representation.

---

# 🎓 Viva Questions

### Why use MediaPipe?

MediaPipe provides hand landmark detection that can be used to derive geometric information about the hand.

### Why use MobileNetV2?

MobileNetV2 provides a lightweight CNN architecture for extracting visual features from images.

### Why combine geometry and CNN features?

Hand geometry captures structural characteristics, while CNN embeddings capture visual information. Combining them provides two complementary representations for matching.

### Why use risk-based authentication?

Different transaction amounts can require different authentication steps. The implementation increases the required factors for higher-value transactions.

### Why use AES encryption?

Biometric templates are sensitive information. Encryption provides protection for the stored biometric data.

### Why use MongoDB?

MongoDB provides a flexible document-oriented data model suitable for storing application and transaction-related data.

### Why use FastAPI?

FastAPI provides an asynchronous Python framework suitable for building high-performance REST APIs and integrating Python-based computer vision and machine-learning components.

---

# 🔮 Future Enhancements

Potential improvements include:

* Liveness detection
* Anti-spoofing mechanisms
* Improved biometric template management
* Docker and Docker Compose deployment
* Cloud deployment
* Centralized monitoring
* Rate limiting
* Advanced fraud detection
* More comprehensive automated testing
* Production-grade key management

---

# ⚠️ Disclaimer

This project is intended for **educational, research, and demonstration purposes**.

Biometric authentication and payment processing in real-world financial systems require additional security controls, regulatory compliance, certified biometric solutions, secure key management, fraud monitoring, and extensive testing before production deployment.

---

# 👨‍💻 Author

**Pradip  Yadav**

Java Backend Developer | Spring Boot | Microservices | Python | AI

* GitHub: [pradipyadav18](https://github.com/pradipyadav18)
* LinkedIn: [Pradip Yadav](https://www.linkedin.com/in/pradip-yadav09/)
* LeetCode: [pradip_yadav_15](https://leetcode.com/u/pradip_yadav_15/)

---

## ⭐ Project

If you find this project useful, consider giving the repository a ⭐ on GitHub.
