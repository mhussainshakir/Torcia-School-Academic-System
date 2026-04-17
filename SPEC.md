# Torcia School & Academic System - PWA

## Project Overview
- **Project Name**: Torcia School & Academic System
- **Type**: Progressive Web App (PWA)
- **Core Functionality**: Centralized platform for school communication, resource sharing, and student management
- **Target Users**: School admins, teachers, and students

## Technical Stack
- **Framework**: Next.js 14 (App Router)
- **Database & Auth**: Firebase (Firestore + Realtime Database)
- **File Storage**: Cloudinary + Google Drive API
- **Styling**: Tailwind CSS + Shadcn UI
- **Hosting**: Vercel (zero-config)

## UI/UX Specification

### Color Palette
- **Primary**: `#3B82F6` (Blue-500)
- **Primary Dark**: `#1E40AF` (Blue-800)
- **Secondary**: `#10B981` (Emerald-500)
- **Accent**: `#F59E0B` (Amber-500)
- **Background Light**: `#FAFAFA` (Neutral-50)
- **Background Dark**: `#0F172A` (Slate-900)
- **Surface Light**: `#FFFFFF`
- **Surface Dark**: `#1E293B` (Slate-800)
- **Text Primary Light**: `#0F172A` (Slate-900)
- **Text Primary Dark**: `#F8FAFC` (Slate-50)
- **Text Secondary**: `#64748B` (Slate-500)
- **Error**: `#EF4444` (Red-500)
- **Success**: `#22C55E` (Green-500)

### Typography
- **Font Family**: `Inter`, sans-serif (from Google Fonts)
- **Heading 1**: 32px, font-weight 700
- **Heading 2**: 24px, font-weight 600
- **Heading 3**: 20px, font-weight 600
- **Body**: 16px, font-weight 400
- **Small**: 14px, font-weight 400
- **Caption**: 12px, font-weight 500

### Spacing System
- **Base unit**: 4px
- **XS**: 4px
- **SM**: 8px
- **MD**: 16px
- **LG**: 24px
- **XL**: 32px
- **2XL**: 48px

### Responsive Breakpoints
- **Mobile**: < 640px
- **Tablet**: 640px - 1024px
- **Desktop**: > 1024px

### Layout Structure
- **Sidebar**: 280px fixed width on desktop, collapsible drawer on mobile
- **Main Content**: Fluid width, max-width 1280px centered
- **Cards**: 16px padding, 8px border-radius, subtle shadow
- **Header**: 64px height, sticky

## Firebase Collections

### `users`
```
id: string (Firebase UID)
email: string
fullName: string
avatarUrl: string
role: 'admin' | 'teacher' | 'student'
googleDriveApiKey: string (optional)
hasDriveApiKey: boolean
classes: string[] (class IDs)
createdAt: timestamp
updatedAt: timestamp
```

### `classes`
```
id: string
name: string (e.g., "Class 9", "Class 10")
description: string
teacherId: string (user ID)
createdAt: timestamp
```

### `attendance`
```
id: string
classId: string
userId: string
date: string (YYYY-MM-DD)
status: 'present' | 'absent'
markedBy: string (user ID)
createdAt: timestamp
```

### `notices`
```
id: string
title: string
content: string
priority: 'normal' | 'urgent'
createdBy: string (user ID)
createdAt: timestamp
```

### Realtime Database
```
/classes/{classId}/messages/{messageId}
  - senderId: string
  - senderName: string
  - senderAvatar: string
  - content: string
  - messageType: 'text' | 'image' | 'pdf' | 'video' | 'youtube'
  - fileUrl: string
  - createdAt: number (timestamp)
```

## Functionality Specification

### 1. Authentication & Onboarding
- Google OAuth login via Firebase Auth
- Email/Password login via Firebase Auth
- Role selection during signup (Admin/Teacher/Student)
- Google Drive API key capture for students (optional)
- Session management with Firebase Auth

### 2. Chat System
- Class-wise chat sections
- Real-time message updates via Firebase Realtime Database
- Image uploads (stored in Cloudinary)
- PDF sharing with download links
- YouTube video embeds (parse URLs to embed)
- Message timestamps and sender info

### 3. Dashboards
**Admin Panel**:
- View all users and classes
- Create/edit classes
- Assign teachers to classes
- System statistics
- Maximum 5 admins allowed

**Teacher Panel**:
- View assigned class members
- Mark attendance
- Create notices
- Remove students from class

**Student Panel**:
- View enrolled class chat
- Upload/view resources
- Access class materials

### 4. PWA Features
- Custom app icon (Torcia flame logo)
- Add to Home Screen prompt
- Service Worker for offline caching
- Manifest.json for installability

### 5. Features
- Attendance tracker (daily toggle)
- Notice board (global announcements)
- Dark/Light mode toggle
- Google Drive integration for file storage

## Component Structure

### /components
- `ui/` - Shadcn UI components
- `auth/firebase/` - Firebase Auth components
- `chat/` - Chat components
- `layout/` - Layout components

### /app (Next.js App Router)
- `/` - Landing page
- `/login` - Login page
- `/register` - Registration page
- `/dashboard` - Role-based dashboard
- `/chat` - Chat list
- `/chat/[classId]` - Class chat
- `/attendance` - Attendance tracking
- `/notices` - Notice board
- `/resources` - Student resources
- `/admin` - Admin dashboard (secret)
- `/setup-admin` - Admin setup page

## API Routes

### Auth
- `/login` - Firebase Auth
- `/register` - Firebase Auth

### Drive (Google Drive API)
- `POST /api/drive/upload` - Upload file
- `POST /api/drive/upload-pdf` - Upload PDF
- `POST /api/drive/upload-image` - Upload image
- `POST /api/drive/upload-video` - Upload video

## Environment Variables

```
# Firebase
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id

# Cloudinary
NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME=your_cloud_name
NEXT_PUBLIC_CLOUDINARY_UPLOAD_PRESET=your_upload_preset

# Google OAuth
NEXT_PUBLIC_GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret

# App
NEXT_PUBLIC_APP_URL=https://your-app.vercel.app
```

## Deployment

### Vercel Zero-Config
1. Push code to GitHub
2. Import project in Vercel
3. Add environment variables
4. Deploy with automatic builds

### Firebase Setup
1. Create Firebase project
2. Enable Authentication (Google + Email)
3. Create Firestore Database
4. Enable Realtime Database
5. Copy configuration to .env.local

### Google Drive API Setup
1. Create Google Cloud project
2. Enable Google Drive API
3. Create API Key
4. Add API Key to app settings

## Implementation Status

All features from the SPEC have been implemented:

- ✅ Authentication with Google OAuth (Firebase)
- ✅ Email/Password Authentication (Firebase)
- ✅ Role-Based Access Control (Admin, Teacher, Student)
- ✅ Real-time Chat System with Firebase Realtime
- ✅ Resource Sharing (Images, PDFs, YouTube embeds) via Cloudinary
- ✅ Google Drive integration for file storage
- ✅ Attendance Tracking
- ✅ Notice Board with priority levels
- ✅ Student Drive API key capture
- ✅ PWA Manifest and offline support
- ✅ Dark/Light mode
- ✅ Responsive design
- ✅ Admin Dashboard (secret)
- ✅ Admin Setup page

See IMPLEMENTATION.md for detailed setup instructions.
