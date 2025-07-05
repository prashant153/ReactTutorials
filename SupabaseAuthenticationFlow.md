# 🛡️ Supabase Authentication Flow – Athlete Edge Admin Portal

This document explains how authentication works in our React app using Supabase, and how routes are protected via React Context and routing logic.

---

## 🎯 Authentication Flow (Step-by-Step Diagram)

```plaintext
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
