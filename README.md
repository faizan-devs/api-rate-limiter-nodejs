# API Rate Limiter System (with API Key, Daily Limits & Email Notifications)

This project implements a robust **API Rate Limiting System** that restricts API usage based on user subscription plans (`basic`, `pro`, `premium`). It features **API key generation**, **daily usage tracking**, **email alerts at 90% and 100% usage**, and **CSV reporting via cron job**.

---

## 📌 Key Features

- 🔐 **API Key Generation** for each user.
- 📊 **Daily API Usage Tracking** per user via MongoDB.
- ⚠️ **Automatic Email Warning** at 90% usage.
- ⛔ **Limit Reached Email** and block at 100% usage.
- 📬 **Daily CSV Usage Report** emailed to admin at 8 AM.
- 🔁 **Usage Reset** every midnight via scheduled cron.
- 🔁 **API Key Regeneration** (admin only).
- 🧾 **User Dashboard (Admin Route)** with daily usage summary.

---

## 🧰 Tech Stack

| Layer       | Tech/Tool                 |
| ----------- | ------------------------- |
| Backend     | Node.js, Express.js       |
| Database    | MongoDB (Mongoose ODM)    |
| Email       | Nodemailer + Gmail SMTP   |
| Scheduler   | node-cron                 |
| Data Format | json2csv (CSV generation) |
| Security    | Hashed API Keys (SHA-256) |

---

## 🧪 API Routes and Usage (Postman Guide)

### 1. Create User (with API Key)

`POST /api/users`

```
{
  "name": "Faizan",
  "email": "faizan@example.com",
  "plan": "basic"
}
```

### 2. Regenerate API Key (Admin)

`PUT /api/admin/users/:id/apikey` <br>
Response will contain a new raw API key (shown once only).

### 3. Rate-Limited Test API Route

`GET /api/data` <br>
Header:

```makefile
x-api-key: YOUR_API_KEY_HERE
```

### 4. Admin Dashboard

`GET /api/admin/dashboard` <br>
📊 Returns all users with today’s usage.

### 5. Update User Details

`PUT /api/users/:id`

```json
{
  "name": "Updated Name",
  "email": "newemail@example.com"
}
```

`scripts/dailyReport.js` <br>
Runs every day at 8 AM (via separate cron or manually) to: <br>
📤 Generate CSV of daily usage <br>
📧 Email it to ADMIN_EMAIL defined in .env

### .env Configuration

Create a `.env` file:

```ini
PORT=3000
MONGO_URI=mongodb+srv://your-db-uri
EMAIL_USER=youremail@gmail.com
EMAIL_PASS=yourapppassword
ADMIN_EMAIL=admin@example.com
```

---

### Future Implementations

- 🔒 JWT-based authentication + role-based access.

- 💰 Stripe integration for upgrading plans.

- 📈 Analytics dashboard with charts (React/Next.js).

- 📬 Admin alerts for users hitting limits.

- 🧪 API testing dashboard (Postman clone UI).

- 📁 Download logs & reports from dashboard.

## Scripts

`npm run dev` – Start dev server (with nodemon)

`node index.js` – Run usage reset cron

`node scripts/dailyReport.js` – Send daily CSV
