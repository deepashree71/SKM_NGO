# 🚀 How to Deploy SKM NGO to Vercel

## Step 1 — Set Up MongoDB Atlas (Free Database)

1. Go to [https://cloud.mongodb.com](https://cloud.mongodb.com) and sign up (free)
2. Click **"Build a Database"** → choose **Free (M0)**
3. Choose a region close to India (e.g. Mumbai)
4. Create a username and password — **save these!**
5. Under **"Where would you like to connect from?"** → choose **"Allow access from anywhere"** (0.0.0.0/0)
6. Click **"Connect"** → **"Connect your application"** → copy the connection string
7. It will look like:
   ```
   mongodb+srv://youruser:yourpassword@cluster0.abc123.mongodb.net/?retryWrites=true&w=majority
   ```
8. Replace `<password>` with your actual password and add the DB name:
   ```
   mongodb+srv://youruser:yourpassword@cluster0.abc123.mongodb.net/ngo_trust?retryWrites=true&w=majority
   ```

---

## Step 2 — Generate a JWT Secret

Open any terminal and run:
```bash
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```
Copy the output — that's your `JWT_SECRET`.

---

## Step 3 — Set Up Razorpay (Test Mode)

1. Go to [https://dashboard.razorpay.com](https://dashboard.razorpay.com) → Sign up
2. Go to **Settings → API Keys → Generate Test Key**
3. Copy **Key ID** (starts with `rzp_test_...`) and **Key Secret**
4. For webhook secret: go to **Settings → Webhooks → Add New Webhook** → set a secret

---

## Step 4 — Set Up Cloudinary (Free Image Hosting)

1. Go to [https://cloudinary.com](https://cloudinary.com) → Sign up free
2. In your Dashboard you'll see: **Cloud Name**, **API Key**, **API Secret**

---

## Step 5 — Set Up Gmail App Password (for email)

1. Go to your Google Account → **Security → 2-Step Verification** (enable it)
2. Then go to **Security → App Passwords**
3. Choose **Mail** → **Other** → type "NGO Trust" → click **Generate**
4. Copy the 16-character password (e.g. `abcd efgh ijkl mnop`)

---

## Step 6 — Push to GitHub

Make sure your code is on GitHub (the repo you already have at deepashree71/SKM_NGO).

```bash
git add .
git commit -m "fix: Vercel deployment fixes"
git push
```

---

## Step 7 — Add Environment Variables in Vercel

1. Go to [https://vercel.com](https://vercel.com) → your project **skm-ngo**
2. Click **Settings → Environment Variables**
3. Add each variable below (select **All Environments**):

| Variable | Value |
|---|---|
| `MONGODB_URI` | Your Atlas connection string from Step 1 |
| `JWT_SECRET` | The random string from Step 2 |
| `JWT_EXPIRES_IN` | `7d` |
| `RAZORPAY_KEY_ID` | `rzp_test_...` from Step 3 |
| `RAZORPAY_KEY_SECRET` | Your Razorpay secret from Step 3 |
| `RAZORPAY_WEBHOOK_SECRET` | Your webhook secret from Step 3 |
| `CLOUDINARY_CLOUD_NAME` | From Step 4 |
| `CLOUDINARY_API_KEY` | From Step 4 |
| `CLOUDINARY_API_SECRET` | From Step 4 |
| `SMTP_HOST` | `smtp.gmail.com` |
| `SMTP_PORT` | `587` |
| `SMTP_USER` | Your Gmail address |
| `SMTP_PASS` | App password from Step 5 (no spaces) |
| `FROM_EMAIL` | `noreply@skm-ngo.vercel.app` |
| `FROM_NAME` | `SKM NGO` |
| `FRONTEND_URL` | `https://skm-ngo.vercel.app` |
| `ADMIN_EMAIL` | `admin@ngotrust.org` |
| `ADMIN_PASSWORD` | Choose a strong password |
| `NODE_ENV` | `production` |
| `VITE_API_BASE_URL` | `https://skm-ngo.vercel.app/api/v1` |
| `VITE_RAZORPAY_KEY_ID` | Same as `RAZORPAY_KEY_ID` |

> **Twilio (WhatsApp/SMS) is optional.** Skip those if you don't need SMS/WhatsApp notifications.

---

## Step 8 — Redeploy

After adding all environment variables:
1. Go to **Deployments** tab in Vercel
2. Click the **three dots** on the latest deployment → **Redeploy**
3. Wait ~2 minutes → visit https://skm-ngo.vercel.app ✅

---

## Step 9 — Seed the Admin Account (one time)

After deployment, run the seed script locally with your production MongoDB URI:

```bash
cd backend
MONGODB_URI="your_atlas_uri_here" node utils/seed.js
```

This creates the admin user with the email/password you set in `ADMIN_EMAIL` / `ADMIN_PASSWORD`.

---

## ⚠️ Important Notes

- **Never commit `.env` files** to GitHub — they're in `.gitignore`
- The `VITE_*` variables are for the **frontend** — add them in Vercel too
- PDF generation (receipts, certificates, ID cards) uses `@sparticuz/chromium` — it works on Vercel's free plan but may be slow (~3-5 seconds) on first use
- File uploads go to **Cloudinary** — not local disk (Vercel has no persistent disk)
