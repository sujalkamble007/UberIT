# RideMate – Real-time Ride Hailing Backend (Node.js/Express)

The Backend powers a real-time ride hailing platform enabling users to book rides, captains to accept and complete them, and both to see live updates. It exposes REST APIs for authentication, maps utilities (via Google Maps), and ride lifecycle, and uses WebSockets for realtime ride status and driver proximity.

## 🔧 Core Capabilities

- **Authentication (JWT)**
  - Users and Captains have separate auth flows: register, login, profile, logout (token blacklist).
- **Maps Integration (Google Maps)**
  - Geocoding: address → coordinates
  - Distance Matrix: distance and ETA between two points
  - Place Autocomplete suggestions
- **Ride Management**
  - Create ride, get fare estimate, confirm/start, live progress, complete ride
- **Realtime Updates (Socket.IO)**
  - Driver availability and proximity
  - Ride status events between user and captain
- **Data Persistence (MongoDB/Mongoose)**
  - Users, Captains (with vehicle and geo index), Rides, Blacklisted tokens

## 🛠 Tech Stack

- **Runtime**: Node.js (Express.js)
- **Database**: MongoDB via Mongoose
- **Auth**: JWT with cookie and header support; blacklist on logout
- **Realtime**: Socket.IO
- **External APIs**: Google Maps (Geocoding, Distance Matrix, Places Autocomplete)

## 📁 Key Folders

- `controllers/` HTTP handlers for users, captains, rides, maps
- `services/` Business logic and integrations (e.g., Google Maps)
- `models/` Mongoose schemas and methods
- `routes/` Express route modules
- `middlewares/` Auth middleware (JWT verify, role-specific)
- `db/` Database connection utility
- `socket.js` Socket.IO setup and event handling
- `app.js` Express app configuration
- `server.js` HTTP server + Socket.IO bootstrap

## 🔑 Environment Variables
Create a `.env` file in `Backend/` with the following:

```env
PORT=3000
DB_CONNECT=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
GOOGLE_MAPS_API=your_google_maps_api_key
```

Notes:
- `DB_CONNECT` is consumed by `db/db.js`.
- `JWT_SECRET` is used in `models/user.model.js`, `models/captain.model.js`, and `middlewares/auth.middleware.js`.
- `GOOGLE_MAPS_API` is used by `services/maps.service.js`.

## ▶️ Scripts

Add or run with `npm` from `Backend/`:

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
}
```

If `nodemon` is not installed globally, add it as a dev dependency:
```bash
npm i -D nodemon
```

## 🚀 Local Development

1. Install dependencies
```bash
cd Backend
npm install
```

2. Configure environment
```bash
cp .env.example .env  # if you create one, otherwise create .env as above
```

3. Start the API
```bash
# Development (recommended)
npm run dev
# or Production
npm start
```

The server will run on `http://localhost:${PORT}` (default `3000`).

## 🔌 API Overview

Base URL: `http://localhost:3000`

- **Users**
  - `POST /users/register`
  - `POST /users/login`
  - `GET /users/profile` (Auth: Bearer or cookie)
  - `GET /users/logout` (Auth)
- **Captains**
  - `POST /captains/register`
  - `POST /captains/login`
  - `GET /captains/profile` (Auth)
  - `GET /captains/logout` (Auth)
- **Maps**
  - `GET /maps/get-coordinates?address=...`
  - `GET /maps/get-distance-time?origin=...&destination=...`
  - `GET /maps/get-suggestions?input=...`
- **Rides**
  - `GET /rides/get-fare?pickup=...&destination=...` (Auth)
  - `POST /rides/create` (Auth)
  - `POST /rides/confirm` (usually captain) (Auth)
  - `GET /rides/start-ride` (Auth)
  - `POST /rides/end-ride` (Auth)

Refer to the route handlers in `controllers/` for request/response details.

## 📡 Socket Events

Socket is initialized in `socket.js` and bound in `server.js`.
Typical events include (names may vary; consult `socket.js`):
- Connection/auth handshake
- Driver location updates
- Ride request/accept/arrive/start/end
- Broadcasts to user/captain rooms

## 🔒 Security
- JWT tokens issued with 24h expiry (see models)
- Blacklist on logout (`models/blacklistToken.model.js`)
- CORS enabled for frontend origin (adjust in `app.js` if needed)

## 🧪 Health Check
- `GET /` returns a simple “Hello World” response

## 🧭 Troubleshooting
- 401/403: Ensure `Authorization: Bearer <token>` is present and valid
- Maps errors: Verify `GOOGLE_MAPS_API` and billing enabled on Google Cloud
- DB errors: Check `DB_CONNECT` and cluster IP Access List
- Socket not connecting: Confirm frontend uses the correct base URL and port

## 📜 License
MIT (or your preferred license)
