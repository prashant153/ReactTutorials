# 🎓 Mini Workshop: React Context

---

## 📌 What is React Context?

**React Context** is like a **global storage** for React components.  
It helps avoid **prop drilling** (passing data through many layers of components).

> Think of it as:  
> “A way to store and access app-wide data from anywhere.”

---

## 🧠 Analogy

```
Without Context:
Grandparent → Parent → Child → Passes data manually layer by layer

With Context:
Grandparent (Provider) → 🛰 → All components access data directly
```

---

## 🔧 How Context Works (4 Steps)

| Step | What You Do                            | Code Example                               |
|------|----------------------------------------|--------------------------------------------|
| 1️⃣   | Create the context                     | `const AuthContext = createContext()`       |
| 2️⃣   | Create a Provider component            | `function AuthProvider({ children }) {...}` |
| 3️⃣   | Wrap your app in the Provider          | `<AuthProvider> <App /> </AuthProvider>`    |
| 4️⃣   | Use `useContext()` in child components | `const { user } = useContext(AuthContext)`  |

---

## 🛠 Let’s Build a Context From Scratch

We'll call it: **ThemeContext** (to toggle dark/light mode)

---

### 🧪 Step 1: Create the Context

```tsx
// src/contexts/ThemeContext.tsx
import { createContext, useContext, useState } from 'react';

export const ThemeContext = createContext({
  isDarkMode: false,
  toggleTheme: () => {},
});
```

---

### 🧪 Step 2: Create the Provider

```tsx
// src/contexts/ThemeContext.tsx
export const ThemeProvider = ({ children }: { children: React.ReactNode }) => {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const toggleTheme = () => setIsDarkMode(prev => !prev);

  return (
    <ThemeContext.Provider value={{ isDarkMode, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

---

### 🧪 Step 3: Wrap Your App

In `src/main.tsx` or `src/App.tsx`:

```tsx
import { ThemeProvider } from './contexts/ThemeContext';

<ThemeProvider>
  <App />
</ThemeProvider>
```

---

### 🧪 Step 4: Use It in a Component

```tsx
// src/components/DarkModeToggle.tsx
import { useContext } from 'react';
import { ThemeContext } from '../contexts/ThemeContext';

export const DarkModeToggle = () => {
  const { isDarkMode, toggleTheme } = useContext(ThemeContext);

  return (
    <button onClick={toggleTheme}>
      {isDarkMode ? '🌙 Dark Mode' : '☀️ Light Mode'}
    </button>
  );
};
```

---

## ✅ Recap: What You’ve Learned

- Context is used to create **shared, global state** in React
- `createContext()` makes a new container
- `useContext()` is how you access that container
- A Provider is what “wraps” your app and gives access to child components

---

## 🧠 Real App Example (AuthContext)

In your app (`AuthContext.tsx`):

```tsx
const { user, signOut } = useContext(AuthContext);

if (!user) {
  return <Navigate to="/auth" />;
}
```

This is how you protect routes using AuthContext.

---

## 🧪 Challenge: Practice It Yourself

Try making your own:

- `RoleContext` — For Admin / Coach access control
- `LanguageContext` — Toggle app language
- `DarkModeContext` — Toggle theme (already shown above)

---

## 📘 Next Up

You can now move on to:
1. `useEffect()` Workshop — How side effects & API calls work
2. Rebuilding `AuthContext.tsx` line-by-line

Ready? Let’s dive in! 🚀
