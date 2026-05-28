# Online Quiz Application

This project is a full-stack single-player online quiz application built using the **MERN stack**. The application allows users to register, log in, complete randomised quiz attempts, review previous attempts, track quiz history, and compare scores on a leaderboard. It also includes a protected admin interface for managing quiz questions.

---

# Features

## Player Features

* User registration and login
* JWT-based authentication
* Protected quiz, history, review, and leaderboard routes
* Randomised quiz questions for each attempt
* Backend score calculation
* Quiz result display after submission
* Persistent quiz history
* Detailed review mode for completed attempts
* Leaderboard based on best attempts
* Dark mode toggle persisted with `localStorage`
* Loading and error feedback during API requests

---

## Admin Features

* Protected admin dashboard
* Role-based admin authorisation
* Create quiz questions
* Edit existing questions
* Delete questions
* Activate/deactivate questions
* Bulk import quiz questions
* Manage optional answer explanations

---

## Security and Reliability Features

* Backend-only quiz marking to prevent answer exposure
* Password hashing with `bcrypt`
* JWT authentication middleware
* Admin-only route protection
* Rate limiting on login and quiz submission endpoints
* `helmet` middleware for secure HTTP headers
* Request sanitisation middleware
* Server-side validation
* Consistent API response envelope

---

# Bonus and Polish Features

## Persistent Review Mode

Completed quiz attempts are saved in the database, allowing users to:

* review attempts immediately after submission
* revisit previous attempts later
* compare selected answers with correct answers
* read explanations attached to questions
* learn from mistakes over time

This extends the approved review-mode variation beyond a temporary result screen.

---

## UI/UX Improvements

* Clear separation between quiz, history, leaderboard, and review pages
* Immediate score feedback after submission
* Correct/incorrect answer indicators during review
* Consistent styling across player and admin pages
* Persistent dark mode support
* Clear loading and error states

---

## Error Handling

The application includes both frontend and backend safeguards, including:

* validation errors for invalid input
* protected route handling
* admin access blocking for non-admin users
* consistent API error responses
* loading/error handling during quiz fetching
* submission failure handling
* rate-limiting feedback for repeated requests

---

# Tech Stack

## Frontend

* React
* React Router
* Axios
* React Context + `useReducer`
* React Hook Form
* Zod
* CSS modules/files
* `localStorage`

---

## Backend

* Node.js
* Express
* MongoDB
* Mongoose
* JWT Authentication
* bcrypt
* express-rate-limit
* helmet

---

# Project Structure

```txt
backend/
  controllers/
  middleware/
  models/
  routes/
  config/
  server.js

frontend/
  src/
    api/
    components/
    context/
    pages/
    App.js
```

---

# Database Models

## User

```js
{
  username: String,
  password: String,
  role: "user" | "admin"
}
```

Passwords are hashed before storage.

---

## Question

```js
{
  questionText: String,
  options: [String],
  correctAnswer: Number,
  explanation: String,
  isActive: Boolean
}
```

* `explanation` supports review mode feedback
* `isActive` allows questions to be disabled without deletion

---

## Score

```js
{
  userId: ObjectId,
  score: Number,
  totalQuestions: Number,
  answers: [
    {
      questionId: ObjectId,
      selectedAnswer: Number,
      isCorrect: Boolean
    }
  ]
}
```

The stored answer list allows completed attempts to be reconstructed later for review mode.

---

# API Response Format

## Success

```json
{
  "success": true,
  "data": {}
}
```

## Error

```json
{
  "success": false,
  "error": "Error message"
}
```

---

# API Routes

## Authentication

```txt
POST /api/auth/register
POST /api/auth/login
```

---

## Quiz

```txt
GET /api/quiz/questions
POST /api/quiz/submit
GET /api/quiz/history
GET /api/quiz/review/:scoreId
```

---

## Leaderboard

```txt
GET /api/leaderboard
```

---

## Admin

```txt
GET /api/admin/questions
POST /api/admin/questions
PUT /api/admin/questions/:id
DELETE /api/admin/questions/:id
PUT /api/admin/questions/:id/toggle
POST /api/admin/questions/import
```

---

# Key Design Decisions

## Backend Quiz Marking

Quiz marking is performed entirely on the backend. The frontend only submits selected answer indexes while the backend retrieves the correct answers from MongoDB and calculates the final score.

This prevents users from accessing correct answers during quiz-taking.

---

## Persistent Review Mode

Completed quiz attempts are stored in the `Score` collection so users can review attempts later through quiz history.

The review page reconstructs:

* selected answers
* correct answers
* correctness status
* explanations

This makes review mode persistent rather than temporary.

---

## Randomised Questions

Questions are shuffled on the backend before being sent to the frontend, making repeated quiz attempts less predictable.

---

## Authentication and Route Protection

JWT authentication protects quiz history, review, leaderboard, and admin routes.

Admin functionality requires both authentication and admin role authorisation.

---

## Rate Limiting

Rate limiting is applied to login and quiz submission endpoints to reduce brute-force login attempts and spam submissions.

---

## Dark Mode Persistence

Dark mode preferences are stored in `localStorage`, allowing theme preferences to persist after page refresh.

---

## Consistent API Envelope

All API responses use the same success/error structure, simplifying frontend error handling and reducing duplicated logic.

---

# Installation and Setup

## Clone the Repository

```bash
git clone <repository-url>
cd COMP4347_ASS2
```

---

# Backend Setup

## Install Dependencies

```bash
cd backend
npm install
```

---

## Create `.env`

```env
PORT=5000
MONGO_URI=<your-mongodb-connection-string>
JWT_SECRET=<your-jwt-secret>
```

---

## Start Backend Server

```bash
npm run dev
```

Backend runs on:

```txt
http://localhost:5000
```

---

# Frontend Setup

## Install Dependencies

```bash
cd frontend
npm install
```

---

## Start Frontend

```bash
npm start
```

Frontend runs on:

```txt
http://localhost:3000
```

API base URL:

```txt
http://localhost:5000/api
```

---

# How to Use the Application

## Player Flow

1. Register or log in
2. Start a quiz
3. Select answers
4. Submit the quiz
5. View the final score
6. Review the completed attempt
7. View previous attempts in history
8. Compare scores on the leaderboard

---

## Admin Flow

1. Log in with an admin account
2. Open the admin dashboard
3. Create, edit, delete, or disable questions
4. Bulk import questions
5. Add explanations for review mode

---

# Testing Checklist

## Authentication

* Register new user
* Login existing user
* Logout functionality
* Protected route blocking
* Admin-only route protection

---

## Quiz Functionality

* Load quiz questions
* Randomise question order
* Submit answers
* Backend score calculation
* Result display after submission

---

## Review Mode

* Review completed attempts
* Display selected answers
* Display correct answers
* Display correctness indicators
* Display explanations

---

## History and Leaderboard

* View quiz history
* Reopen previous attempts
* Display leaderboard rankings
* Use best attempt for leaderboard scores

---

## Admin Features

* Create questions
* Edit questions
* Delete questions
* Activate/deactivate questions
* Bulk import questions

---

## UI/UX and Error Handling

* Dark mode persistence
* Validation error handling
* Login rate limiting
* Quiz submission rate limiting
* Loading states
* Error states
* Consistent API error responses

