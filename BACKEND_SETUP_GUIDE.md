# Campus Clubs Portal - Backend Setup Guide

## Database Models

### 1. User Model (`backend/models/User.js`)
Stores student and admin information with:
- Basic info: name, email, rollNo, password
- Profile: phone, department, year, profilePhoto
- Role-based access: student/admin
- Activity tracking: sports stats, NSS hours, NCC rank
- Badges and achievements

### 2. Profile Model (`backend/models/Profile.js`)
Extended profile information:
- Stores editable profile details
- Activity statistics for each section
- Privacy code for media access

### 3. Booking Model (`backend/models/Booking.js`)
Ground and equipment bookings:
- Facility selection
- Date/time scheduling
- Status tracking (pending, approved, rejected)
- User contact information

### 4. Media Model (`backend/models/Media.js`)
Photos and videos storage:
- Media metadata (title, type, URL)
- Event association
- Category (sports, nss, ncc)
- Upload tracking

### 5. MediaAccess Model (`backend/models/MediaAccess.js`)
Privacy code management:
- 6-digit privacy codes
- Student ID association
- Access tracking

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new student
- `POST /api/auth/login` - Login (generates privacy code)
- `POST /api/auth/refresh` - Refresh access token
- `GET /api/auth/me` - Get current user
- `PUT /api/auth/profile` - Update profile
- `PUT /api/auth/change-password` - Change password

### Profile Management
- `GET /api/profile/:userId` - Get user profile
- `PUT /api/profile/:userId` - Update profile (syncs to database)
- `GET /api/profile/:userId/transcript` - Download activity transcript

### Bookings
- `POST /api/bookings` - Create booking request
- `GET /api/bookings/user/:userId` - Get user bookings
- `GET /api/bookings` - Get all bookings (admin)
- `PUT /api/bookings/:bookingId/status` - Update booking status (admin)

### Media Gallery
- `POST /api/media/verify-access` - Verify 6-digit privacy code
- `GET /api/media/gallery` - Get public media
- `POST /api/media/upload` - Upload media (admin)
- `GET /api/media/category/:category` - Get media by category

## Privacy Code System

### How It Works
1. When a student registers or logs in, a unique 6-digit privacy code is generated
2. This code is stored in the `MediaAccess` collection
3. To access the media gallery, students must enter their privacy code
4. Once verified, they can view all event photos and videos
5. Code changes with each login for security

### Privacy Code Benefits
- Prevents unauthorized access to event media
- Personal code for each student
- Changes regularly for enhanced security
- Tracks last access time

## Setting Up Environment Variables

Create a `.env` file in the `backend` folder:

\`\`\`env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/campus-clubs-portal

# JWT Tokens
JWT_SECRET=your-super-secret-jwt-key-change-this
JWT_EXPIRE=7d
JWT_REFRESH_SECRET=your-refresh-secret-key-change-this
JWT_REFRESH_EXPIRE=30d

# Frontend
FRONTEND_URL=http://localhost:3000

# Email (Optional)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
EMAIL_FROM=noreply@campusclubs.com
\`\`\`

## Running the Backend

### 1. Install MongoDB
- Download from https://www.mongodb.com/try/download/community
- Install and keep MongoDB running

### 2. Install Dependencies
\`\`\`bash
cd backend
npm install
\`\`\`

### 3. Start Backend Server
\`\`\`bash
npm run dev
\`\`\`

Expected output:
\`\`\`
✅ Connected to MongoDB
🚀 Campus Clubs Portal API running on port 5000
📱 Environment: development
\`\`\`

### 4. Test Backend
\`\`\`bash
# Check if API is running
curl http://localhost:5000/api/health

# Should return:
# {
#   "status": "OK",
#   "message": "Campus Clubs Portal API is running",
#   "timestamp": "2024-01-15T10:30:00.000Z"
# }
\`\`\`

## Data Flow

### Profile Update Flow
1. User edits profile in frontend (`/dashboard/profile`)
2. Frontend sends PUT request to `/api/profile/:userId`
3. Backend updates both User and Profile collections
4. Data persists in MongoDB
5. Frontend receives confirmation

### Booking Flow
1. User selects facility, date, time in `/dashboard/sports/booking`
2. Frontend sends POST to `/api/bookings`
3. Backend creates Booking document with "pending" status
4. Admin reviews bookings in admin panel
5. Admin approves/rejects via `/api/bookings/:id/status`
6. Booking status updates in database

### Media Gallery Flow
1. User visits `/dashboard/media`
2. If not authenticated, shows privacy code prompt
3. User enters 6-digit code received on login
4. Frontend sends POST to `/api/media/verify-access`
5. Backend verifies code in MediaAccess collection
6. If valid, user can view all media
7. Admin uploads media via `/api/media/upload`

## Database Collections

### users
\`\`\`json
{
  "_id": ObjectId,
  "name": "John Doe",
  "email": "john@example.com",
  "rollNo": "CS21B001",
  "department": "CSE",
  "year": 3,
  "phone": "9876543210",
  "sportsStats": { "matchesPlayed": 12, "matchesWon": 8 },
  "nssStats": { "totalHours": 45 },
  "nccStats": { "currentRank": "Cadet" },
  "badges": [],
  "createdAt": ISODate,
  "updatedAt": ISODate
}
\`\`\`

### profiles
\`\`\`json
{
  "_id": ObjectId,
  "userId": ObjectId,
  "name": "John Doe",
  "phone": "9876543210",
  "department": "CSE",
  "year": "3rd Year",
  "bio": "Passionate about sports and volunteering",
  "privacyCode": "123456",
  "createdAt": ISODate
}
\`\`\`

### bookings
\`\`\`json
{
  "_id": ObjectId,
  "type": "ground",
  "facility": { "name": "Football Ground", "capacity": 22 },
  "startTime": ISODate,
  "endTime": ISODate,
  "bookedBy": ObjectId,
  "purpose": "Football practice",
  "status": "pending",
  "contactNumber": "9876543210",
  "createdAt": ISODate
}
\`\`\`

### media
\`\`\`json
{
  "_id": ObjectId,
  "title": "Sports Day 2024",
  "url": "https://...",
  "type": "photo",
  "eventName": "Annual Sports Meet",
  "category": "sports",
  "uploadedBy": ObjectId,
  "uploadedDate": ISODate
}
\`\`\`

### mediaaccess
\`\`\`json
{
  "_id": ObjectId,
  "studentId": ObjectId,
  "privacyCode": "654321",
  "isActive": true,
  "lastAccessedAt": ISODate,
  "createdAt": ISODate
}
\`\`\`

## Testing Endpoints with Curl

### Register
\`\`\`bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "Password123",
    "rollNo": "CS21B001",
    "department": "CSE",
    "year": 3
  }'
\`\`\`

### Login
\`\`\`bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "Password123"
  }'
\`\`\`

### Update Profile
\`\`\`bash
curl -X PUT http://localhost:5000/api/profile/USER_ID \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d '{
    "name": "Jane Doe",
    "phone": "9876543210",
    "department": "ECE",
    "year": "4"
  }'
\`\`\`

### Create Booking
\`\`\`bash
curl -X POST http://localhost:5000/api/bookings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d '{
    "facility": "football-ground",
    "date": "2024-01-20",
    "time": "04:00 PM - 06:00 PM",
    "purpose": "Football practice",
    "participants": "20",
    "contactPerson": "John Doe",
    "phone": "9876543210"
  }'
\`\`\`

### Verify Media Access
\`\`\`bash
curl -X POST http://localhost:5000/api/media/verify-access \
  -H "Content-Type: application/json" \
  -d '{
    "privacyCode": "123456",
    "studentId": "USER_ID"
  }'
\`\`\`

## Troubleshooting

### MongoDB Connection Error
- Ensure MongoDB is installed and running
- Check MONGODB_URI in .env
- On Windows: MongoDB should auto-start
- On Mac/Linux: Run `mongod` in terminal

### Port 5000 Already in Use
\`\`\`bash
# Find process using port 5000
lsof -i :5000
# Kill the process
kill -9 PID
\`\`\`

### JWT Token Errors
- Ensure JWT_SECRET and JWT_REFRESH_SECRET are set in .env
- Keep tokens secure, don't share them
- Check token expiration

### CORS Errors
- Ensure FRONTEND_URL matches your frontend URL
- Default is http://localhost:3000
- Update if frontend runs on different port

## Next Steps

1. Connect frontend to backend API endpoints
2. Test all features in development
3. Deploy MongoDB to cloud (MongoDB Atlas)
4. Deploy backend to hosting (Heroku, Railway, Vercel)
5. Update FRONTEND_URL and MONGODB_URI for production
6. Enable email notifications
7. Setup file upload to Cloudinary
