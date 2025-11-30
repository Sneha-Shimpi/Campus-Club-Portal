# Campus Clubs Portal - Complete Project Documentation

## Overview
This is a **Full-Stack Web Application** built with:
- **Frontend:** React.js + Next.js + TypeScript + TailwindCSS
- **Backend:** Node.js + Express.js + JavaScript
- **Database:** MongoDB + Mongoose
- **Authentication:** JWT Tokens

---

## PROJECT FOLDER STRUCTURE

\`\`\`
campus-clubs-portal/
│
├── app/                                  # Frontend Pages (Next.js App Router)
│   ├── page.tsx                          # LOGIN & REGISTRATION PAGE
│   ├── layout.tsx                        # Main Layout (Fonts, Providers)
│   ├── globals.css                       # Global Styles & Design Tokens
│   │
│   ├── dashboard/                        # Student Dashboard Section
│   │   ├── layout.tsx                    # Dashboard Layout with Sidebar
│   │   ├── page.tsx                      # Dashboard Main Page (Stats & Calendar)
│   │   ├── profile/
│   │   │   └── page.tsx                  # Profile Page (Editable Details)
│   │   ├── sports/
│   │   │   ├── page.tsx                  # Sports Main Page (Matches & Leaderboard)
│   │   │   └── booking/
│   │   │       └── page.tsx              # Sports Booking Page (Calendar & Ground)
│   │   ├── nss/
│   │   │   ├── page.tsx                  # NSS Volunteer Activities Page
│   │   │   └── log-hours/
│   │   │       └── page.tsx              # Log Volunteer Hours Page
│   │   ├── ncc/
│   │   │   ├── page.tsx                  # NCC Training Page (Ranks & Attendance)
│   │   │   └── quiz/
│   │   │       └── page.tsx              # NCC Quiz Page
│   │   └── media/
│   │       └── page.tsx                  # Media Gallery with Privacy Code
│   │
│   └── admin/                            # Admin Dashboard Section
│       ├── layout.tsx                    # Admin Layout with Sidebar
│       ├── page.tsx                      # Admin Dashboard (Analytics)
│       ├── events/
│       │   └── page.tsx                  # Manage Events
│       ├── students/
│       │   └── page.tsx                  # Manage Students
│       └── analytics/
│           └── page.tsx                  # Advanced Analytics & Reports
│
├── components/                           # Reusable React Components
│   ├── auth/
│   │   └── auth-context.tsx              # Authentication Context (Login/Register Logic)
│   ├── dashboard/
│   │   └── sidebar.tsx                   # Dashboard Sidebar Navigation
│   ├── admin/
│   │   └── sidebar.tsx                   # Admin Sidebar Navigation
│   ├── ui/                               # Pre-built UI Components
│   │   ├── button.tsx                    # Button Component
│   │   ├── card.tsx                      # Card Component
│   │   ├── input.tsx                     # Input Field Component
│   │   ├── tabs.tsx                      # Tabs Component
│   │   ├── badge.tsx                     # Badge Component
│   │   ├── modal.tsx                     # Modal/Dialog Component
│   │   └── ... other UI components
│   └── theme-provider.tsx                # Theme Switching Provider
│
├── backend/                              # Node.js + Express Backend
│   ├── server.js                         # Main Server File (Express Setup)
│   ├── package.json                      # Backend Dependencies
│   ├── .env                              # Environment Variables (MongoDB URI, JWT Secrets)
│   │
│   ├── config/
│   │   └── database.js                   # MongoDB Connection Setup
│   │
│   ├── models/                           # MongoDB Schemas
│   │   ├── User.js                       # User Schema (name, email, roll number, role)
│   │   ├── Profile.js                    # Profile Schema (editable user details)
│   │   ├── Booking.js                    # Booking Schema (ground/equipment bookings)
│   │   ├── SportsMatch.js                # Sports Match Schema
│   │   ├── NSSActivity.js                # NSS Activity Schema
│   │   ├── NCCTraining.js                # NCC Training Schema
│   │   ├── Quiz.js                       # Quiz Schema
│   │   ├── Media.js                      # Media Schema (photos/videos)
│   │   └── MediaAccess.js                # Media Access Schema (privacy codes)
│   │
│   ├── routes/                           # API Endpoints
│   │   ├── auth.js                       # Authentication Routes (Register, Login, Refresh)
│   │   ├── profile.js                    # Profile Routes (Get, Update, Download Transcript)
│   │   ├── bookings.js                   # Booking Routes (Create, Approve, View)
│   │   ├── sports.js                     # Sports Routes (Matches, Leaderboard, Results)
│   │   ├── nss.js                        # NSS Routes (Activities, Hours, Verification)
│   │   ├── ncc.js                        # NCC Routes (Training, Attendance, Ranks, Quizzes)
│   │   ├── media.js                      # Media Routes (Upload, Access with Privacy Code)
│   │   └── admin.js                      # Admin Routes (User Management, Analytics)
│   │
│   ├── middleware/
│   │   ├── auth.js                       # JWT Verification Middleware
│   │   └── validation.js                 # Request Validation Middleware
│   │
│   └── utils/
│       ├── emailService.js               # Email Sending Service
│       ├── tokenUtils.js                 # JWT Token Generation/Refresh
│       └── responseHelper.js             # Standardized API Response Helper
│
├── public/                               # Static Files (Images)
│   └── images/
│       └── diverse-students-studying.png # Sample Image
│
├── package.json                          # Frontend Dependencies
├── tsconfig.json                         # TypeScript Configuration
└── README.md                             # Project Instructions

\`\`\`

---

## WHERE EACH FEATURE IS LOCATED

### 1. LOGIN & REGISTRATION PAGE
**File:** `app/page.tsx`
**What it does:**
- Displays beautiful login/registration interface
- Modern gradient design with purple, pink, red colors
- Two tabs: "Sign In" and "Register"
- Validates email and password
- Calls backend `/api/auth/register` and `/api/auth/login` endpoints

**Code breakdown:**
- Uses `useAuth()` hook to manage login/register state
- Form submission calls `login()` or `register()` functions
- On success, redirects to `/dashboard`
- Uses Tabs component to switch between login and register

---

### 2. AUTHENTICATION SYSTEM
**File:** `components/auth/auth-context.tsx`
**What it does:**
- Creates React Context for global authentication state
- Manages user login/logout/register
- Stores JWT token in localStorage
- Provides `useAuth()` hook for any component to access user data

**Backend Route:** `backend/routes/auth.js`
- **POST `/api/auth/register`** - Create new account
- **POST `/api/auth/login`** - Login user (returns JWT token + privacy code)
- **POST `/api/auth/refresh`** - Refresh expired token

**Database:** `backend/models/User.js`
- Stores: name, email, password (hashed), roll number, role (student/admin)
- On login: generates 6-digit privacy code

---

### 3. STUDENT DASHBOARD
**File:** `app/dashboard/page.tsx`
**What it does:**
- Shows student stats (matches played, volunteer hours, drills attended)
- Displays upcoming events in calendar
- Shows recent achievements and badges
- Navigation sidebar to access other sections

**Sidebar:** `components/dashboard/sidebar.tsx`
- Links to Profile, Sports, NSS, NCC, Media Gallery
- Logout option

---

### 4. PROFILE PAGE
**File:** `app/dashboard/profile/page.tsx`
**What it does:**
- Shows student details: name, photo, email, roll number, department
- Edit button to modify profile
- Save button sends data to backend
- Download activity transcript as PDF

**Backend Route:** `backend/routes/profile.js`
- **GET `/api/profile/:userId`** - Get user profile
- **PUT `/api/profile/:userId`** - Update profile
- **GET `/api/profile/:userId/transcript`** - Download PDF transcript

**Database:** `backend/models/Profile.js`
- Stores: name, phone, department, year, bio, photo URL
- Links to User collection

---

### 5. SPORTS CLUB SECTION
**File:** `app/dashboard/sports/page.tsx`
**What it does:**
- Shows match schedules and results
- Displays leaderboard with top players
- Shows statistics (wins, losses, goals)

**Booking Page:** `app/dashboard/sports/booking/page.tsx`
**What it does:**
- Calendar picker to select date (working calendar)
- Select ground/equipment to book
- Enter booking details
- Submit to backend

**Backend Route:** `backend/routes/sports.js`
- **GET `/api/sports/matches`** - Get all matches
- **POST `/api/sports/bookings`** - Create booking
- **GET `/api/sports/leaderboard`** - Get player rankings
- **PUT `/api/sports/matches/:id/results`** - Update match results (admin)

**Database:** 
- `backend/models/SportsMatch.js` - Match schedules, results, teams
- `backend/models/Booking.js` - Ground/equipment bookings with dates

---

### 6. NSS VOLUNTEER SECTION
**File:** `app/dashboard/nss/page.tsx`
**What it does:**
- Shows volunteer opportunities to signup
- Displays total volunteer hours
- Shows impact metrics (trees planted, blood donated, etc.)
- Lists achievements and badges

**Log Hours Page:** `app/dashboard/nss/log-hours/page.tsx`
**What it does:**
- Form to log volunteer activity and hours
- Upload proof (photo/document)
- Submit for admin verification

**Backend Route:** `backend/routes/nss.js`
- **GET `/api/nss/activities`** - Get volunteer opportunities
- **POST `/api/nss/log`** - Log volunteer hours
- **PUT `/api/nss/log/:id/verify`** - Admin verifies hours (admin only)
- **GET `/api/nss/impact`** - Get impact metrics
- **GET `/api/nss/leaderboard`** - Top volunteers

**Database:** `backend/models/NSSActivity.js`
- Stores: activity type, hours, date, proof, status (pending/verified)
- Tracks: trees planted, blood donated, people helped

---

### 7. NCC TRAINING SECTION
**File:** `app/dashboard/ncc/page.tsx`
**What it does:**
- Shows training schedules (drills and theory)
- Displays cadet rank and progression
- Shows attendance record
- Displays achievements and certificates

**Quiz Page:** `app/dashboard/ncc/quiz/page.tsx`
**What it does:**
- Interactive quiz for NCC cadets
- Multiple choice questions
- Shows score after completion
- Tracks quiz attempts

**Backend Route:** `backend/routes/ncc.js`
- **GET `/api/ncc/trainings`** - Get training schedules
- **POST `/api/ncc/attendance`** - Mark attendance (QR code scanning)
- **GET `/api/ncc/ranks`** - Get cadet ranks
- **POST `/api/ncc/quizzes/:id/attempt`** - Submit quiz answers
- **GET `/api/ncc/quizzes/:id`** - Get quiz questions
- **PUT `/api/ncc/cadets/:id/promote`** - Promote cadet rank (admin only)

**Database:** 
- `backend/models/NCCTraining.js` - Training sessions, attendance
- `backend/models/Quiz.js` - Quiz questions, answers, scores

---

### 8. MEDIA GALLERY (WITH PRIVACY CODE)
**File:** `app/dashboard/media/page.tsx`
**What it does:**
- Shows event photos and videos
- Requires 6-digit privacy code to access
- Displays privacy code from login
- Grid/List view to browse media

**Privacy Code System:**
- Generated during login
- Unique for each student
- Stored in database `MediaAccess` collection
- Required to view gallery

**Backend Route:** `backend/routes/media.js`
- **POST `/api/media/verify-access`** - Verify privacy code
- **GET `/api/media`** - Get all media (if verified)
- **POST `/api/media/upload`** - Upload photo/video (admin only)
- **DELETE `/api/media/:id`** - Delete media (admin only)

**Database:** 
- `backend/models/Media.js` - Stores photo/video metadata and URLs
- `backend/models/MediaAccess.js` - Stores privacy codes and access logs

---

### 9. ADMIN DASHBOARD
**File:** `app/admin/page.tsx`
**What it does:**
- Shows overview analytics
- Charts for sports wins, volunteer hours, cadets trained
- Quick stats

**Admin Sidebar:** `components/admin/sidebar.tsx`
- Links to: Events, Students, Analytics, Reports

**Events Management:** `app/admin/events/page.tsx`
- Create, edit, delete events
- Upload event photos/videos
- Manage event schedules

**Students Management:** `app/admin/students/page.tsx`
- View all students
- Approve/reject participants
- Assign roles
- View attendance records

**Analytics & Reports:** `app/admin/analytics/page.tsx`
- Charts showing:
  - Top players in sports
  - Total volunteer hours
  - Cadet attendance rates
  - NCC rank distribution

**Backend Route:** `backend/routes/admin.js`
- **GET `/api/admin/users`** - Get all users
- **PUT `/api/admin/users/:id/role`** - Change user role
- **GET `/api/admin/analytics`** - Get analytics data
- **DELETE `/api/admin/users/:id`** - Delete user (super admin only)
- **POST `/api/admin/events`** - Create event
- **PUT `/api/admin/events/:id`** - Edit event
- **DELETE `/api/admin/events/:id`** - Delete event

---

## DATABASE STRUCTURE (MongoDB)

### Collections & Schemas

**1. Users Collection**
\`\`\`javascript
{
  _id: ObjectId,
  name: "John Doe",
  email: "john@college.edu",
  password: "hashed_password",
  rollNo: "CS21B001",
  role: "student", // or "admin"
  privacyCode: "123456", // 6-digit code for media access
  createdAt: Date,
  updatedAt: Date
}
\`\`\`

**2. Profiles Collection**
\`\`\`javascript
{
  _id: ObjectId,
  userId: ObjectId (reference to User),
  name: "John Doe",
  phone: "9876543210",
  department: "Computer Science",
  year: "3rd",
  photo: "url_to_photo",
  bio: "I am...",
  badgesEarned: ["Best Player", "Gold Volunteer"],
  updatedAt: Date
}
\`\`\`

**3. Bookings Collection**
\`\`\`javascript
{
  _id: ObjectId,
  userId: ObjectId (reference to User),
  facilityType: "ground", // or "equipment"
  facilityName: "Football Ground",
  date: Date,
  timeSlot: "10:00-12:00",
  purpose: "Practice match",
  status: "pending", // or "approved", "rejected"
  createdAt: Date
}
\`\`\`

**4. SportsMatches Collection**
\`\`\`javascript
{
  _id: ObjectId,
  sport: "Cricket",
  date: Date,
  team1: ["userId1", "userId2"],
  team2: ["userId3", "userId4"],
  result: {
    winner: "team1",
    score: "15-10"
  },
  status: "completed" // or "scheduled", "ongoing"
}
\`\`\`

**5. NSSActivities Collection**
\`\`\`javascript
{
  _id: ObjectId,
  userId: ObjectId (reference to User),
  activity: "Tree Planting",
  hours: 8,
  date: Date,
  proof: "url_to_photo",
  status: "verified", // or "pending"
  impact: {
    treesPlanted: 50,
    bloodDonated: 0,
    peopleHelped: 100
  }
}
\`\`\`

**6. NCCTraining Collection**
\`\`\`javascript
{
  _id: ObjectId,
  userId: ObjectId (reference to User),
  trainingType: "Drill", // or "Theory"
  date: Date,
  attended: true,
  rank: "Recruit", // or "Lance Corporal", etc.
  performanceScore: 85,
  certificateUrl: "url_to_certificate"
}
\`\`\`

**7. Quizzes Collection**
\`\`\`javascript
{
  _id: ObjectId,
  title: "NCC Discipline Quiz",
  questions: [
    {
      question: "What is...",
      options: ["A", "B", "C", "D"],
      correctAnswer: "A"
    }
  ],
  timeLimit: 30 // minutes
}
\`\`\`

**8. Media Collection**
\`\`\`javascript
{
  _id: ObjectId,
  title: "Sports Day 2024",
  type: "photo", // or "video"
  url: "url_to_image_or_video",
  category: "sports", // or "nss", "ncc"
  uploadedBy: ObjectId (reference to Admin),
  uploadedAt: Date
}
\`\`\`

**9. MediaAccess Collection**
\`\`\`javascript
{
  _id: ObjectId,
  userId: ObjectId (reference to User),
  privacyCode: "123456",
  accessGrantedAt: Date,
  expiresAt: Date
}
\`\`\`

---

## HOW FRONTEND & BACKEND CONNECT

### Example: User Login Process

**1. User clicks "Sign In" on app/page.tsx**
\`\`\`
User enters email & password
↓
Clicks "Sign In →" button
↓
handleLogin() function is called
↓
Calls login() from auth-context.tsx
\`\`\`

**2. Frontend sends data to Backend**
\`\`\`
POST http://localhost:5000/api/auth/login
{
  email: "john@college.edu",
  password: "password123"
}
\`\`\`

**3. Backend (backend/routes/auth.js) processes request**
\`\`\`
- Receives request
- Finds user in MongoDB
- Compares password hash
- Generates JWT token
- Generates 6-digit privacy code
- Returns token + privacy code
\`\`\`

**4. Backend response sent back to Frontend**
\`\`\`
{
  token: "eyJhbGc...",
  refreshToken: "eyJhbGc...",
  user: {
    id: "123",
    name: "John Doe",
    role: "student"
  },
  privacyCode: "123456"
}
\`\`\`

**5. Frontend stores data and redirects**
\`\`\`
- Stores token in localStorage
- Saves user data in React Context
- Shows privacy code to user
- Redirects to /dashboard
\`\`\`

---

## HOW TO RUN EVERYTHING

**Terminal 1 - MongoDB**
\`\`\`bash
mongod
# Output: waiting for connections on port 27017
\`\`\`

**Terminal 2 - Backend**
\`\`\`bash
cd backend
npm install  # First time only
npm run dev
# Output: Server running on port 5000
\`\`\`

**Terminal 3 - Frontend**
\`\`\`bash
npm install  # First time only
npm run dev
# Output: Local: http://localhost:3000
\`\`\`

---

## FILE NAMING CONVENTIONS

- **Pages:** `page.tsx` (Next.js convention)
- **Layouts:** `layout.tsx` (Next.js convention)
- **Components:** PascalCase like `Sidebar.tsx`, `LoginForm.tsx`
- **Hooks:** camelCase with "use" prefix like `useAuth.ts`
- **Utilities:** camelCase like `emailService.js`, `tokenUtils.js`
- **Styles:** Global in `globals.css`, component styles in TailwindCSS classes

---

## API ENDPOINT SUMMARY

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/auth/register` | Create new account |
| POST | `/api/auth/login` | Login user |
| POST | `/api/auth/refresh` | Refresh token |
| GET | `/api/profile/:userId` | Get user profile |
| PUT | `/api/profile/:userId` | Update profile |
| GET | `/api/sports/matches` | Get matches |
| POST | `/api/sports/bookings` | Create booking |
| GET | `/api/nss/activities` | Get activities |
| POST | `/api/nss/log` | Log volunteer hours |
| GET | `/api/ncc/trainings` | Get trainings |
| POST | `/api/media/verify-access` | Verify privacy code |
| GET | `/api/admin/analytics` | Get analytics |

---

## TECHNOLOGY STACK SUMMARY

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend UI | React.js 18 | Component library |
| Frontend Framework | Next.js 15 | Full-stack React framework |
| Frontend Language | TypeScript | Type-safe JavaScript |
| Styling | TailwindCSS v4 | Utility-first CSS |
| Backend Runtime | Node.js | JavaScript runtime |
| Backend Framework | Express.js | Web server |
| Backend Language | JavaScript (ES6+) | Server-side code |
| Database | MongoDB | NoSQL database |
| Database Driver | Mongoose | MongoDB ODM |
| Authentication | JWT | Token-based auth |
| Password Security | bcryptjs | Password hashing |

---

## KEY CONCEPTS

**JWT Authentication:**
- User logs in → Backend generates JWT token
- Token stored in localStorage on frontend
- Every API request includes token in Authorization header
- Backend verifies token before processing request
- If token expires → Use refresh token to get new token

**Privacy Code System:**
- Generated when user logs in
- 6 random digits
- Required to access media gallery
- Changes with each login for security
- Stored in MediaAccess collection with expiration

**Role-Based Access:**
- Student: Can view own profile, participate in activities, view media
- Admin/Coordinator: Can manage events, approve bookings, view analytics

---

## NEXT STEPS FOR CUSTOMIZATION

1. **Change Project Name:**
   - Edit `app/layout.tsx` - metadata title
   - Edit `app/page.tsx` - header and branding

2. **Add Your College Logo:**
   - Replace emoji (🎓) with your logo
   - Place image in `public/images/`

3. **Customize Colors:**
   - Edit color tokens in `app/globals.css`
   - Change gradients in component files

4. **Add More Features:**
   - Follow same pattern as existing features
   - Create new route in backend
   - Create new page in frontend
   - Add database model if needed

---

This documentation explains your entire full-stack project. Now you understand where each feature is, how it works, and how everything connects!
