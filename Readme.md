\# ExploreWithRJ – System Architecture \& Secure Payment Design



This repository documents the system design and architectural decisions behind ExploreWithRJ, a serverless study abroad consultation platform.



The focus of this documentation is backend architecture, secure payment workflows, database modeling, and scalability principles.



---



\## 🏗 High-Level Architecture



```text

User (Browser)

&nbsp;   ↓

React Frontend (Vite)

&nbsp;   ↓

Netlify Serverless Functions (Node.js)

&nbsp;   ↓

MongoDB Atlas

```



The backend acts as the authoritative source of truth for:



\- Authentication

\- Authorization

\- Payment verification

\- Data integrity enforcement



---



\## 💳 Secure Payment Architecture (Paystack)



Payments are implemented using a backend-enforced, webhook-verified model.



\### Transaction Flow



```text

Client → Backend (Initialize Transaction)

&nbsp;      → Paystack Checkout

&nbsp;      → Paystack → Webhook → Backend Verification

&nbsp;      → MongoDB Update

```



\### Security Measures



\- HMAC-SHA512 signature verification

\- `x-paystack-signature` header validation

\- `crypto.timingSafeEqual` to prevent timing attacks

\- Backend-only state transitions

\- No client-side payment trust



Bookings are marked as completed only after verified webhook confirmation.



---



\## 🔐 Authentication \& Authorization



\- Stateless JWT authentication

\- Role-Based Access Control (RBAC)

\- Backend-enforced route protection

\- Secure password hashing



All authorization checks occur server-side.



---



\## 🗄 Database Design



MongoDB Atlas is used with structured schema modeling and performance-focused indexing.



\### Core Collections



\- users

\- services

\- appointments

\- scholarships

\- scholarship\_favorites

\- payments



\### Indexing Strategy



\- Unique email index for users

\- Compound indexes for filtering \& sorting

\- Full-text search index for scholarships

\- Indexed foreign key references

\- Payment status indexing for transaction queries



This indexing strategy reduced dashboard query latency by 25%.



---



\## 🧠 Key Engineering Principles



\- The backend is the source of truth

\- The client is untrusted

\- Payments must be cryptographically verified

\- Authorization must be enforced server-side

\- Stateless systems scale horizontally

\- Data integrity > UI assumptions



---



\## 🎯 Purpose of This Repository



This repository exists to document architectural thinking and backend system design decisions.



The production source code remains private, but the system design and security principles are documented here for transparency and technical discussion.

