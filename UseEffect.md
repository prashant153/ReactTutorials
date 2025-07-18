# 🧪 React `useEffect()` Mini-Workshop

Welcome to the **React `useEffect()` Mini-Workshop**! This guide will help you **understand, visualize, and implement** `useEffect()` with confidence using a series of hands-on examples and mental models.

---

## 📘 What is `useEffect()`?

`useEffect()` is a **React Hook** that lets you perform **side effects** in function components.

> **Side effects** = things React doesn’t do automatically  
> (e.g., data fetching, event listeners, timers, subscriptions, logging, etc.)

---

## 🧠 Conceptual Understanding

- `useEffect()` runs **after the render** is committed to the screen.
- It can be configured to run:
  - Only after **initial mount**
  - After **specific prop or state changes**
  - On **every render**
- You can also write a **cleanup function** inside it using `return`.

---

## 🔁 Basic Syntax

```js
useEffect(() => {
  // This is your effect logic (runs after render)

  return () => {
    // This is your optional cleanup logic
  };
}, [dependencies]);
```

---

## ✅ Workshop Goals

1. Understand when and how `useEffect()` runs
2. Learn how to control its behavior with dependencies
3. Explore cleanup function behavior
4. Build a simple Timer App using `useEffect()`

---

## 🧪 Step 1: Run on Mount Only

```js
useEffect(() => {
  console.log('Component mounted!');
}, []);
```

- ✅ Runs once when component mounts
- ❌ Won’t run again
- 🧹 Cleanup (if any) runs on unmount

---

## 🔁 Step 2: Run on State or Prop Change

```js
useEffect(() => {
  console.log('Count changed to:', count);
}, [count]);
```

- ✅ Runs when `count` changes
- 🧹 Cleanup runs **before** re-running if you return a cleanup

---

## ⚠️ Step 3: Run on Every Render (Rare)

```js
useEffect(() => {
  console.log('Runs on every render!');
});
```

- ✅ Runs after every render
- ❌ Avoid unless necessary (e.g., real-time sync)

---

## 🧹 Step 4: Cleanup Function Example

```js
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Tick');
  }, 1000);

  return () => {
    clearInterval(timer); // Cleanup the interval
  };
}, []);
```

- ✅ Sets up a timer on mount
- 🧹 Cleans up the timer on unmount to prevent memory leaks

---

## 🛠 Mini Project: Timer App

### ✅ Features:
- Start timer on mount
- Display seconds
- Stop timer on unmount

### 🧩 Code:

```jsx
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    return () => {
      clearInterval(interval); // Cleanup
    };
  }, []);

  return <h1>Timer: {seconds}s</h1>;
}
```

Try adding a button to **unmount** the component and see if the interval stops (check DevTools console).

---

## 🧭 Summary Cheat Sheet

| Scenario            | Effect Runs? | Cleanup Runs? |
|---------------------|--------------|----------------|
| Mount               | ✅ Yes       | ❌ No          |
| Dependency change   | ✅ Yes       | ✅ Yes (before re-run) |
| Unmount             | ❌ No effect | ✅ Yes         |

---

## 💡 Tips

- Only put **side-effect logic** in `useEffect()`, not rendering logic.
- Always include **cleanup** when you set up subscriptions, listeners, or intervals.
- Use the **dependency array** to control frequency and optimize performance.

---
Happy Learning! 🚀
