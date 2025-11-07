# Food ordering web app (backend)

Lightweight Express + TypeScript backend for a food-ordering application used for a semester project. It exposes REST endpoints for restaurants, users, orders, promotions and reviews, and includes development helpers for Stripe webhook testing and local development.

## Table of contents
- [Features](#features)
- [Quick start](#quick-start)
- [Environment variables](#environment-variables)
- [Scripts](#scripts)
- [API overview](#api-overview)
- [Testing](#testing)
- [Project structure](#project-structure)
- [Contributing](#contributing)

## Features
- REST API implemented with Express + TypeScript
- MongoDB (Mongoose) models for restaurants, orders, promotions, reviews and users
- Stripe webhook forwarding helper for local development
- Input validation and authentication middleware (JWT)
- Unit / integration test scaffolding with Jest + Supertest

## Quick start

Requirements
- Node.js 18+ (or a recent LTS)
- npm (bundled with Node)

1. Clone the repository and install dependencies:

```powershell
cd \food-ordering-web-app
npm install
```

2. Create a `.env` file in the project root (see the Environment variables section below).

3. Run the development server (starts the app and Stripe listen helper):

```powershell
npm run dev
```

4. Build the TypeScript sources and run the compiled app:

```powershell
npm run build
npm run start
```

Notes
- `npm run dev` uses `concurrently` to run `nodemon` and the Stripe listener defined in the scripts — this helps forward webhooks to your local server for testing.

## Environment variables

Create a `.env` file with the variables your app expects. The exact names used in your code may vary; common variables for this project include:

- `MONGO_URI` — MongoDB connection string
- `JWT_SECRET` — secret used to sign JSON Web Tokens
- `STRIPE_SECRET_KEY` — Stripe secret key
- `STRIPE_WEBHOOK_SECRET` — Stripe webhook signing secret (used for verification)
- `CLOUDINARY_URL` or cloudinary credentials (if image upload is enabled)

Make sure to never commit `.env` to the repository. Add it to `.gitignore` if not already ignored.

## Scripts

The `package.json` scripts available in this project are:

- `npm run dev` — runs `nodemon` (restarts server on changes) and the Stripe forwarder concurrently
- `npm run stripe` — runs `stripe listen --forward-to localhost:7000/api/order/checkout/webhook` (used by `dev`)
- `npm run build` — installs dependencies and compiles TypeScript (`npx tsc`)
- `npm run start` — starts the compiled Node app from `dist/index.js`

Use `npm run build` before `npm run start` in production environments.

## API overview

This backend exposes endpoints for the following resources (route files located in `src/routes`):

- Restaurants: `src/routes/RestaurantRoute.ts` and `src/routes/MyRestaurantRoute.ts`
- Users: `src/routes/MyUserRoute.ts`
- Orders: `src/routes/OrderRoute.ts` (includes checkout/webhook handling)
- Promotions: `src/routes/PromotionRoute.ts`
- Reviews: `src/routes/ReviewRoute.ts`

Examples (adjust host/port as needed):

```
GET  http://localhost:7000/api/restaurant
POST http://localhost:7000/api/order/checkout
POST http://localhost:7000/api/order/checkout/webhook  <-- used by Stripe forwarding
```

Refer to the route files in `src/routes` and their corresponding controllers in `src/controllers` for request/response details and schema validation.

## Testing

This project includes Jest for tests. Run tests with:

```powershell
npm test
```

If you need to run individual test files you can use `npx jest <path-to-test>` or the VS Code Jest extension.

## Project structure

Top-level folders of interest:

- `src/controllers` — request handlers and business logic
- `src/routes` — Express route definitions
- `src/models` — Mongoose models (restaurant, order, promotion, review, user)
- `src/middleware` — auth and validation middleware
- `testing` — Postman collection and manual test artifacts

## Contributing

1. Open an issue describing the bug or feature.
2. Create a branch `feat/your-feature` or `fix/issue-123`.
3. Implement changes, add tests where appropriate.
4. Open a pull request with a clear description.

## Troubleshooting

- If the server fails to start, check `.env` and ensure `MONGO_URI` is correct.
- If Stripe webhooks are not received locally, confirm `npm run dev` is running and that the Stripe CLI is authenticated.

## Contact

For questions about this semester project, open an issue or contact the project maintainer.
