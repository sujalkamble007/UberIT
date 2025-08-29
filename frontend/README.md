# UberIT – React Frontend (Vite + Tailwind + Socket.IO)

The Frontend is a React application that enables users to search locations, request rides, and track drivers in real time, and allows captains to manage ride flows. It connects to the backend via REST and Socket.IO and integrates Google Maps.

## 🔧 Core Features

- **Auth Flows** for Users and Captains (login, signup, logout, protected routes)
- **Location Search** with Google Places Autocomplete
- **Fare Estimation** before ride creation
- **Ride Lifecycle**: request → looking for driver → waiting → riding → finish
- **Realtime Tracking** using Socket.IO
- **Responsive UI** with Tailwind CSS

## 🛠 Tech Stack

- **React 18**, **Vite**
- **Tailwind CSS**
- **Axios** for API calls
- **React Router** for navigation
- **Socket.IO Client** for realtime
- **@react-google-maps/api** for Google Maps components

## 📁 Notable Structure

- `src/pages/` Screens like `Home`, `Riding`, `CaptainHome`, auth wrappers, etc.
- `src/components/` UI components like `LiveTracking`, `ConfirmRidePopUp`, `FinishRide`, etc.
- `src/context/` React Context for `UserContext`, `CapatainContext`, and `SocketContext`
- `src/App.jsx`, `main.jsx` app bootstrap

## 🔑 Environment Variables
Create `frontend/.env` (Vite requires the `VITE_` prefix):

```env
VITE_BASE_URL=http://localhost:3000
VITE_API_URL=http://localhost:3000
VITE_GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

Notes:
- `VITE_BASE_URL` is used for most REST calls and Socket.IO base in multiple files.
- `VITE_API_URL` is used in logout endpoints in some pages; keep it aligned with `VITE_BASE_URL`.
- `VITE_GOOGLE_MAPS_API_KEY` is used by `LiveTracking` via `@react-google-maps/api`.

If you deploy, update these accordingly.

## ▶️ Scripts

From `frontend/`:

```bash
npm run dev      # Start Vite dev server
npm run build    # Production build
npm run preview  # Preview production build
npm run lint     # Lint the project
```

## 🚀 Local Development

1. Install dependencies
```bash
cd frontend
npm install
```

2. Configure environment
```bash
# Create .env with the variables above
```

3. Start the app
```bash
npm run dev
```

The app will run on the Vite default port (e.g., `http://localhost:5173`). Ensure the backend is running and CORS is allowed.

## 🔌 Integration Points

- REST API Base: `${import.meta.env.VITE_BASE_URL}` in pages/components for login, signup, rides, maps
- Socket Base: `${import.meta.env.VITE_BASE_URL}` in `context/SocketContext.jsx`
- Google Maps API Key: `VITE_GOOGLE_MAPS_API_KEY` in `components/LiveTracking.jsx`

## 🧭 Troubleshooting

- 404/Network errors: Verify `VITE_BASE_URL` points to your backend server and that it is running
- CORS issues: Confirm backend CORS configuration
- Google Maps errors: Ensure the API key is valid and correct services are enabled
- Socket not connecting: Check base URL, port, and that backend Socket.IO is initialized

## 📜 License
MIT (or your preferred license)
