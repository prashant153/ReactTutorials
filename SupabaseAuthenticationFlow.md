# 🛡️ Supabase Authentication Flow – Athlete Edge Admin Portal

This document explains how authentication works in our React app using Supabase, and how routes are protected using React Context and route guards.

---

## 🎯 Authentication Flow (Step-by-Step Diagram)

```
[1] User → opens app in browser
             |
             ↓
[2] App loads <AuthProvider> → Checks if a session exists via Supabase client
             |
             ↓
  ┌──────────────────────────────┐
  │ If session exists:           │
  │ - Set user in AuthContext    │
  │ - Allow access to dashboard  │
  └──────────────────────────────┘
             |
             ↓
  ┌──────────────────────────────┐
  │ If no session:               │
  │ - Redirect to /auth route    │
  │   (login/signup page)        │
  └──────────────────────────────┘
             |
             ↓
[3] User enters email & password
             |
             ↓
[4] AuthContext → calls Supabase client API:
     supabase.auth.signInWithPassword({ email, password })
             |
             ↓
[5] Supabase returns:
     ✔ success → a session + user info
     ✖ error   → wrong credentials
             |
             ↓
[6] On success:
     - Store session in memory
     - Supabase auto-saves it in localStorage
     - User is redirected to `/`
             |
             ↓
[7] All protected routes now work:
     - <ProtectedRoute> checks AuthContext
     - Shows dashboard, sidebar, etc.
```

---

## 🔐 Logout Flow

```
User clicks "Sign Out"
  ↓
signOut() from AuthContext calls:
  → supabase.auth.signOut()
  ↓
Supabase clears session
  ↓
User is redirected to /auth
```

---

## 🧠 Behind the Scenes

### ✅ Supabase Handles:
- ✅ User management in `auth.users`
- ✅ Session persistence via `localStorage`
- ✅ Automatic session refreshing
- ✅ Token storage (access & refresh)
- ✅ Auth state change subscriptions (via `onAuthStateChange`)

All of this is abstracted by Supabase’s client library.

---

## 🔁 Auto-Create Profile via Trigger

When a user signs up, we want to automatically create a profile row for them.

```
1. New user signs up → added to auth.users
2. Supabase trigger fires:
     INSERT INTO profiles (id, email) VALUES (new.id, new.email)
3. Profiles table now has a row for the user
4. Supabase policies ensure:
     - Each user can read/write only their own profile
```

This happens via:
- SQL trigger + function (in your migration file)
- Row-Level Security (RLS) policies

---

## 🧰 Files Responsible for This Flow

| File Path                                | Role in Auth Flow                           |
|------------------------------------------|---------------------------------------------|
| `src/contexts/AuthContext.tsx`           | Global React Context for login/logout/state |
| `src/components/auth/ProtectedRoute.tsx` | Route guard: redirects unauthenticated users|
| `src/components/pages/Auth.tsx`          | Sign-in and Sign-up UI + logic              |
| `src/components/layout/Sidebar.tsx`      | Displays current user + Sign Out button     |
| `supabase/migrations/....sql`            | Creates `profiles` table, RLS, and trigger  |
| `src/integrations/supabase/types.ts`     | Typed interface for Supabase tables         |

---

## ✅ What Happens on Sign Up

1. User signs up via Supabase Auth
2. Supabase stores:
   - Email
   - Hashed password
   - Unique `user.id`
3. A Postgres **trigger** inserts a new row in `profiles`
4. RLS policies ensure each user can only access their row
5. Frontend receives session + user → redirects to dashboard
6. AuthContext stores the session globally
7. ProtectedRoute now gives access to dashboard and more

---