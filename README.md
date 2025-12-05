# AI-Powered-RFP-Management_Systems
A single-user web application that streamlines the end-to-end RFP (Request for Proposal) process using AI. Users can create RFPs in natural language, manage vendors, send RFPs via email, automatically parse vendor responses, and compare proposals with AI-assisted recommendations.
# 1. Project Overview
Many procurement processes are slow, error-prone, and repetitive due to unstructured data from emails, PDFs, and free-form vendor responses. This project:

Converts natural language RFP descriptions into structured JSON.

Maintains vendor master data and sends RFPs via email.

Automatically parses messy vendor responses using AI.

Compares proposals and recommends vendors based on pricing, delivery, terms, and completeness
# 2. Features

RFP Creation: Describe what you need in natural language, automatically parsed into structured RFPs.

Vendor Management: Store vendors, categorize them, and select vendors to send RFPs.

Email Integration: Send RFPs and receive vendor responses automatically via SMTP/IMAP.

Proposal Parsing: Extract pricing, delivery, warranty, and terms using AI.

Comparison & Recommendation: AI-assisted proposal evaluation with summary, ranking, and recommended vendor.

# 3. Architecture
```
Frontend (React + Vite + MUI)
       |
       v
Backend (Node.js + Express)
       |
       +--> PostgreSQL (Prisma ORM)
       |
       +--> OpenAI GPT-4 / GPT-4.1-mini
       |
       +--> Email (SMTP send, IMAP receive)
 ```
Flow: User → Create RFP → Select Vendors → Send Email → Vendor Replies → AI Parses Proposal → Compare & Recommend.

# 4. Tech Stack

Frontend: React, Vite, Material UI, Axios, React Router

Backend: Node.js, Express, dotenv

Database: PostgreSQL + Prisma ORM

Email: Nodemailer (SMTP) + imap-simple (IMAP)

AI: OpenAI GPT-4.1-mini (parsing), GPT-4.1 (comparison)

Dev Tools: Prisma Migrate, Vite Dev Server

# 5. Setup & Installation
Backend
```
cd backend
npm install
npx prisma migrate dev --name init
npm run dev
```

Frontend
```
cd frontend
npm install
npm run dev
```


Backend runs on http://localhost:4000

Frontend runs on http://localhost:5173

# 6. Environment Variables

Copy .env.example → .env and fill in:

```
OPENAI_API_KEY=
SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASS=
IMAP_HOST=
IMAP_USER=
IMAP_PASS=
DATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/rfpdb"
FRONTEND_URL=http://localhost:5173
```

# 7. Database Schema

Tables:

RFP

id, title, descriptionRaw, structured (JSON), budget, deliveryDays, paymentTerms, warrantyYears, items (JSON), status, createdAt

Vendor

id, name, email, category, notes

Proposal

id, rfpId, vendorId, rawEmail, parsed (JSON), totalPrice, deliveryDays, warrantyYears, paymentTerms, aiConfidence, score, createdAt

Relationships:

RFP 1:N Proposal

Vendor 1:N Proposal

# 8. API Documentation
RFPs

POST /api/rfps/parse – Convert natural language → structured JSON

POST /api/rfps – Create new RFP

GET /api/rfps/:id – View RFP

POST /api/rfps/:id/send – Send RFP to selected vendors

Vendors

GET /api/vendors – List vendors

POST /api/vendors – Add vendor

Proposals

GET /api/proposals/rfp/:id – List proposals for RFP

GET /api/proposals/rfp/:id/compare – AI-assisted comparison

# 9. Decisions & Assumptions

Single-user workflow; no authentication.

RFP and proposal data stored as JSON for flexibility.

Vendor emails assumed to include reference to the correct RFP.

AI heavily used for parsing free-form text and making recommendations.

Simplified email workflow: no attachment parsing (can be extended).

# 10. AI Integration

RFP Parsing: GPT-4.1-mini converts natural language into structured JSON.

Proposal Parsing: GPT-4.1-mini extracts pricing, delivery, warranty, and terms from free-text emails.

Proposal Comparison: GPT-4.1 ranks vendors and generates summary + recommendation.

Prompts designed for structured JSON output to minimize parsing errors.
  /src

