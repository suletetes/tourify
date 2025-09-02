# Tourify

Tourify is a full-stack Node.js tour booking application built with Express, MongoDB (Mongoose) and Pug. It includes REST APIs for tours, users, reviews and bookings, server-side rendered views, Stripe integration for payments, and email notifications.

## Features

- REST API for tours, users, reviews and bookings
- Server-side rendered pages with Pug (overview, tour details, login, account)
- Authentication with JWT, secure cookies and password reset via email
- File upload and image processing (Sharp)
- Stripe integration for checkout and webhook support
- Security middlewares: helmet, rate limiting, data sanitization, XSS protection, HPP
- Static front-end assets (CSS/JS) and client-side scripts

## Showcase

### Showcase images

View the screenshots below (files are in `showcaseImages/`):

| Tours | Tour First View |
|:---:|:---:|
| <img src="showcaseImages/Tours.png" alt="Tours" width="420" /> | <img src="showcaseImages/tour%20firstview.png" alt="Tour first view" width="420" /> |

| Tour Second View | Tour Third View | Tour Fourth View |
|:---:|:---:|:---:|
| <img src="showcaseImages/tour%20secondView.png" alt="Tour second view" width="300" /> | <img src="showcaseImages/tour%20thirdView.png" alt="Tour third view" width="300" /> | <img src="showcaseImages/tour%20fourthView.png" alt="Tour fourth view" width="300" /> |

| Login | Profile |
|:---:|:---:|
| <img src="showcaseImages/login.png" alt="Login" width="360" /> | <img src="showcaseImages/profile.png" alt="Profile" width="360" /> |

## Quick Start

1. Clone the repo and install dependencies:

   npm install

2. Create a `config.env` in the project root (or copy `example.config.env`) and fill in required values (see Environment variables below).

3. Run the app in development:

   npm run dev

   Or start normally:

   npm start

4. Open the site in your browser (default port defined in `config.env`, fallback 3000):

   http://localhost:3000

## Environment variables

The app expects environment variables defined in `config.env` (or set in the environment). Fill these with your own credentials/values:


```env
NODE_ENV=development
PORT=3000
DATABASE=your-mongodb-connection-string
DATABASE_PASSWORD=your-mongodb-password
JWT_SECRET=your-jwt-secret
JWT_EXPIRES_IN=90d
JWT_COOKIE_EXPIRES_IN=90
STRIPE_SECRET_KEY=your-stripe-secret-key
STRIPE_WEBHOOK_SECRET=your-stripe-webhook-secret
EMAIL_FROM=your-email@example.com
SENDGRID_USERNAME=your-sendgrid-username
SENDGRID_PASSWORD=your-sendgrid-password
EMAIL_HOST=your-email-host
EMAIL_PORT=your-email-port
EMAIL_USERNAME=your-email-username
```
Note: `DATABASE` should be the full MongoDB connection string and can include the placeholder `<PASSWORD>` which will be replaced by `DATABASE_PASSWORD` at runtime.

## Available scripts

- npm install — install dependencies
- npm run dev — run server in development (uses server.js)
- npm start — start server
- npm run start:prod — start in production (NODE_ENV=production)
- npm run debug — start with debugger (ndb)
- npm run watch:js / npm run build:js — build/watch front-end JS with Parcel

## Project structure (high level)

- app.js — Express app and middleware
- server.js — server bootstrap and DB connection
- controllers/ — route handlers for API and views
- models/ — Mongoose models (Tour, User, Review, Booking)
- routes/ — Express routes
- views/ — Pug templates for server-rendered pages
- public/ — static assets (css, js, images)
- dev-data/ — sample data and import script
- utils/ — helpers (email, errors, API features)
- showcaseImages/ — demo screenshots for README/presentation

## Postman collection

A Postman collection is included: `tourify-postman-collection.json` for testing the API.

## Notes

- Fill `config.env` with real credentials before starting the app.
- For Stripe webhooks, run the webhook endpoint `/webhook-checkout` and configure Stripe to send events to your server (or use the Stripe CLI for local testing).
- The app uses Pug templates and static files under `public/`. The example data in `dev-data/` can be imported using `dev-data/data/import-dev-data.js`.

## Seeding the database (dev-data)

This project includes a ready-to-use dev-data folder with sample data and a small import script to seed or remove that data from your MongoDB database.

What is included

- `dev-data/data/import-dev-data.js` — script that connects to the database (reads `config.env`) and either imports or deletes sample data.
- `dev-data/data/tours.json` — sample tour documents
- `dev-data/data/users.json` — sample user accounts (the import script bypasses validation for users)
- `dev-data/data/reviews.json` — sample reviews
- `dev-data/img/` and `dev-data/templates/` — sample images and Pug templates used for development and testing

Step-by-step: how to run the seed (import) or delete the sample data

1. Ensure you have a working MongoDB and valid connection string:
   - If you use MongoDB Atlas, get the connection string and put it in `config.env` as `DATABASE` with the placeholder `<PASSWORD>` where your password should go.
   - Set `DATABASE_PASSWORD` in `config.env` to the DB user's password.

2. Install dependencies (from project root):

   npm install

3. Verify `config.env` at project root contains at least:

```env
DATABASE=your-mongodb-connection-string-with-<PASSWORD>
DATABASE_PASSWORD=your-mongodb-password
NODE_ENV=development
```

4. Run the import script to seed the database (from project root):

   node dev-data/data/import-dev-data.js --import

   Expected output on success (example):
   - "DB connection successful!"
   - "Data successfully loaded!"

5. Remove the sample data (if needed):

   node dev-data/data/import-dev-data.js --delete

   Expected output on success (example):
   - "DB connection successful!"
   - "Data successfully deleted!"

Notes and warnings

- The import script calls `process.exit()` when it finishes — it connects using `dotenv` and the `config.env` file located in the project root.
- The script creates users with validation disabled: `User.create(users, { validateBeforeSave: false })`. The sample users are for development/testing only.
- Only run these commands against development or test databases. Do not run them against production data.
- If you see connection errors, ensure your `DATABASE` string is correct and that your IP is allowed (for Atlas) or local MongoDB is running.

## Learned from

this is what i have learn and build from https://www.udemy.com/course/nodejs-express-mongodb-bootcamp/
Node.js, Express, MongoDB & More: The Complete Bootcamp
