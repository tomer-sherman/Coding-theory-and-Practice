# ⚛️ React `useEffect` Guide

If you do not understand the `useState` concept, go over it here: [useState (Local State) Guide].

Before understanding what `useEffect` is, we want to understand what a **Side-effect** is.

## 🌪️ What is a Side-effect?

An action that has an effect on an entity _outside_ the component itself.

**For Example:**

- The command `setInterval`, which is connected to the browser window API, not the React component itself.
    
- An action in Component A that changes Component B.
    
- A component that wants to fetch data from an external server or database.
    

### ⚠️ Why are they called side effects?

Because if you just drop those actions in the main script and don't contain them in a classic function or inside a `useEffect()` box, they will activate every single time the component **renders**.

If a side-effect updates state, it triggers a re-render, which triggers the side-effect again, creating an infinite loop that overloads the system and causes a crash.

## 🛡️ How to Contain Side Effects

There are 2 ways to contain side effects:

1. **Inside an event handler function:** A function called upon an HTML event (like a button click), as long as it doesn't have a recursion that breaks the system.
    
2. **Inside a `useEffect()` hook.**
    

## 🛠️ How to use `useEffect`

You contain the side-effect within the `useEffect` block.

TypeScript

```
useEffect(() => {
  // 📍 Here lies the side-effect.
}, [])
```

### 👀 The Dependency Array `[]`

- The empty array at the end of the effect box means the `useEffect` only activates **once** on the initial render of the component. It is not listening to any changes in variables.
    
- If, for example, the array contained `[a, b, c]`, those variables would be watched by this `useEffect` box. Every time one of them changes, the `useEffect` runs again.
    

> 💡 **Note:** `useEffect` usually comes with `useState` — they almost always run together!

## ⏱️ Code Example: The Infinite Clock

This example shows a clock that starts at zero and runs continuously.

TypeScript

```
import { useState, useEffect } from "react";

export function ClockComponent() {
  const [time, setTime] = useState<number>(0);

  useEffect(() => {
    // 1. The Side Effect: setting up the interval
    const timer = setInterval(() => {
      setTime(sec => sec + 1);
    }, 1000); // Runs every 1000 milliseconds (1 second)

    // 2. The Cleanup Function: 
    // This unmounting line is essential. Without it, React Strict Mode 
    // will trigger this timer twice and make it jump 2 seconds at a time.
    return () => clearInterval(timer);
    
  }, []); // Empty array = only setup the timer on first render

  return(
    <span>CLOCK: {time}</span>
  );
};
```

### 🌊 Flow of Code:

1. **First Render:** The user renders this component for the first time. The time shown on screen is `0`. `useEffect` triggers **exactly once**, and inside it, a `setInterval` starts ticking every 1 second.
    
2. **Continuous Updates:** Every second, `setTime` is activated by the internal timer. The component re-renders and shows the new `time + 1`. `useEffect` makes sure that `setInterval` doesn't activate every second—it activates only once, creating a single, stable internal clock.
    

### 💥 What if we didn't use `useEffect`?

Without using `useEffect`, every time the component renders, it creates a separate timeline and new clocks.

Because every time `setTime` is activated, the component renders again. If you threw `setInterval` in the script _without_ `useEffect`, it would create a new clock every second. You would be running infinite clocks like a giant recursion: The first render creates 1 internal clock, the second creates 2, then 4, then 8... eventually crashing the system.