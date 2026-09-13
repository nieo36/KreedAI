

## Overview

KreedAI is a full-stack sports performance app utilizing browser-based MediaPipe Pose estimation to process real-time video frames, calculate joint angles, track movement velocity, and evaluate biomechanical form across fitness, basketball, boxing, and weightlifting modules. 

The architecture consists of a React/TypeScript frontend (PWA with IndexedDB offline storage) and a Node.js/Express REST API backed by MongoDB and Redis for authentication, assessment synchronization, sports passport generation, and recruiter candidate filtering.

---

## Setup & Running

### 1. Environment Setup

**`backend/.env`**:
```env
PORT=2000
HOST=0.0.0.0
MONGO_URI=mongodb://localhost:27017/kreedai
JWT_ACCESS_SECRET=your_jwt_access_secret
JWT_REFRESH_SECRET=your_jwt_refresh_secret
JWT_EMAIL_SECRET=your_jwt_email_secret
CLIENT_URL=http://localhost:5173
```

**`client/.env`**:
```env
VITE_API_URL=http://localhost:2000/api/v1
```

---

### 2. Run Locally

```bash
# Install all dependencies
npm run install-services

# Start Backend (http://localhost:2000)
cd backend && npm start

# Start Frontend (http://localhost:5173)
cd client && npm run dev
```

---

### 3. Run with Docker

```bash
docker-compose up --build -d
```

---

## API Endpoints (`/api/v1`)

### Authentication (`/auth`)
- `POST /auth/register` - User registration
- `POST /auth/login` - User login
- `GET  /auth/verify-email` - Email verification token handler
- `POST /auth/refresh` - Refresh access token via HTTP cookie
- `POST /auth/logout` - Clear cookies and revoke refresh token
- `POST /auth/forgot-password` - Trigger password reset email
- `POST /auth/reset-password` - Reset password with token
- `GET  /auth/google` & `/auth/google/callback` - OAuth 2.0 flow
- `POST /auth/2fa/setup` & `/auth/2fa/verify` - Two-factor authentication
- `PUT  /auth/profile` - Update athlete/recruiter profile

### Assessment (`/assessment`)
- `POST /assessment/sync` - Sync individual workout result
- `POST /assessment/batch-sync` - Sync queued offline workouts
- `GET  /assessment/history` - Fetch workout logs history
- `GET  /assessment/stats` - Fetch aggregated athletic performance metrics

### Leaderboard (`/leaderboard`)
- `GET /leaderboard/` - Global and sport-specific athlete rankings
- `GET /leaderboard/my-position` - Current user's ranking stats

### Recruiter (`/recruiter`)
- `GET /recruiter/candidates` - Search and filter athlete profiles

---

## Database & Status Diagnostics

- **Database**: MongoDB connected via Mongoose (`User` and `Assessment` models).
- **Cache/Session**: Redis for session caching and rate-limiting.
- **Connection Checks**:
  ```bash
  cd backend
  node test_db.js     # Check MongoDB connection
  node test_auth.js   # Verify Auth utilities
  node test_login.js  # Verify Login flow
  ```
