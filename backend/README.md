# NGO Trust Digital Platform — Backend API

Node.js / Express / MongoDB REST API — Version 4.0

## Quick Start

```bash
cd backend
cp .env.example .env
# Fill in your MongoDB URI, JWT secret, Razorpay keys, etc.

npm install
npm run seed    # seed admin, programs, events, stories, gallery
npm run dev     # development with nodemon
npm start       # production
```

API runs at: `http://localhost:5000/api/v1`

## Project Structure

```
backend/
├── server.js                  # Entry point
├── config/
│   └── db.js                  # MongoDB connection
├── middleware/
│   └── auth.js                # JWT protect + adminOnly
├── models/
│   ├── User.js
│   ├── Volunteer.js
│   ├── Donation.js
│   ├── DonationPoster.js
│   ├── Program.js
│   ├── Event.js
│   ├── EventRegistration.js
│   ├── Certificate.js
│   ├── Gallery.js
│   ├── Story.js
│   ├── Notification.js
│   ├── NotificationPreference.js
│   ├── AuditLog.js
│   ├── TransparencyDoc.js
│   ├── ImpactLocation.js
│   ├── CmsPage.js
│   └── ContactMessage.js
├── routes/
│   ├── auth.js
│   ├── donations.js
│   ├── donationPosters.js
│   ├── programs.js
│   ├── volunteers.js
│   ├── events.js
│   ├── certificates.js
│   ├── gallery.js
│   ├── stories.js
│   ├── transparency.js
│   ├── contact.js
│   ├── impactMap.js
│   ├── notifications.js
│   ├── cms.js
│   └── admin.js               # All /admin/* endpoints
└── utils/
    ├── pdfGen.js              # Puppeteer PDF generation
    ├── notify.js              # Nodemailer + Twilio
    ├── auditLog.js            # Admin action logging
    └── seed.js                # Database seeder
```

## Environment Variables

| Variable | Description |
|---|---|
| `PORT` | Server port (default 5000) |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET` | Secret for JWT signing |
| `JWT_EXPIRES_IN` | Token expiry (default 7d) |
| `RAZORPAY_KEY_ID` | Razorpay key ID |
| `RAZORPAY_KEY_SECRET` | Razorpay key secret |
| `RAZORPAY_WEBHOOK_SECRET` | Razorpay webhook secret |
| `SMTP_HOST` | SMTP host for emails |
| `SMTP_USER` | SMTP username |
| `SMTP_PASS` | SMTP password |
| `TWILIO_ACCOUNT_SID` | Twilio SID (optional) |
| `TWILIO_AUTH_TOKEN` | Twilio token (optional) |
| `FRONTEND_URL` | Frontend URL for CORS & links |
| `ADMIN_EMAIL` | Seed admin email |
| `ADMIN_PASSWORD` | Seed admin password |

## Default Admin (after seed)
- Email: `admin@ngotrust.org`
- Password: `Admin@1234`

## Key API Routes

| Method | Route | Auth |
|---|---|---|
| POST | /auth/register | Public |
| POST | /auth/login | Public |
| GET  | /auth/me | JWT |
| POST | /donations/create | JWT |
| POST | /donations/webhook | Razorpay Sig |
| GET  | /certificates/verify/:token | Public |
| GET  | /admin/stats | Admin |
| POST | /admin/certificates/bulk-issue | Admin |
| POST | /admin/broadcast | Admin |

## PDF Generation
All PDFs are generated on-demand using Puppeteer (server-side HTML→PDF). No files are permanently stored — the PDF buffer is streamed directly to the client. Puppeteer requires Chrome — on Linux servers run:
```bash
apt-get install -y chromium-browser
```
