## 🧠 Understanding States

State refers to the **local component state**, in which you manage your component's variables. You can use an analogy in Native JS for scopes:

- 🌍 **Global Scope:** In JS, when you manage your variables in the main script.
    
- 🏠 **Local Scope:** When you work inside a function or a loop, you could say you work in the Local Scope of the function.
    

### 🏠 Local State

Taking this analogy and moving it to React: While working on a specific component, **Local State Management** is managing your inner variables of that _specific_ component.

### 🌍 Global State

While **Global State** is the management of the variables across the whole site or portions of it.

## 🔄 React Rendering

In React, rendering is **per component**.

If you change a variable inside a specific component and if you code it properly, **only this specific component and its nested child components** will be re-rendered again.

> 🚀 **Why this matters:** This makes the app run smoother, not rendering the whole HTML again just for a single variable!

**HOW DO YOU MANAGE VARIABLES TO DO IT?** 👇

## 🛠️ The `useState` Command

You use the command `useState` to manage these variables.

### 📝 How to use:

You assign a two-item array to the function `useState(initial load var)`.

- **Types:** You tell it what kind of type it works with inside the `< >`.
    
- **Initial Render:** Then inside the round brackets `( )` you tell it what the initial variable is that you want to load on the component the first time it renders in.
    

### 📦 The Array Breakdown: `[first, setFirst]`

- 🏷️ `first` **is the name of the variable.**
    
- ⚙️ `setFirst` **is a function** that activates once you pass an argument inside `setFirst(SOME COMMAND)` to change the variable.
    

After this function runs, it **re-renders** the component and shows the new variable inside the component. Inside the argument of `setFirst(x => x + 1)`, `x` represents the current value of `first` (`x === first`).

⚡ `setFirst` usually goes inside a function, like a regular function that is activated by HTML events, or functions that activate inside `useEffect` (which we will talk about in a second).

## 💻 Simple Code Example

Let's take a simple example of the use of the `useState` function, in which there is a button, and every time you press it, a variable in the component gets `+1` added to it. 

TypeScript

```
import { useState } from "react";

export function SomeComponent() {
  // 📍 Here usually sits the Local State Management:
  const [num, setNum] = useState<number>(1);
  
  // num is the var 
  // setNum is a function that once you activate it rerenders the page. 
  // inside the (initial render var).

  function btnPress(): void {
    setNum(num + 1);
  };

  return(
    <button onClick={btnPress}>
      PRESS ME!!! {num}
    </button>
  );
};
```

## 🌊 The Rendering Flow

**Let's look at the flow of code when the component first renders:**

1. 👀 **Initial View:** On the screen, you will see only the button and the `num`, which is equal to **1**.
    
2. 🖱️ **The Click:** Then you press the button, and this is what happens next:
    
3. ➕ **The Function Activates:** The function that you activated, `setNum`, takes `num` and adds `+1` to it.
    
4. ♻️ **The Re-render:** The return section re-renders, and the new value of `num` is calculated as `num + 1`, which is **2**.
    
5. ✅ **The Result:** Since it re-renders the component, `num` is now equal to **2** in the upper section. Thus returning `num` in the lower return section as **2**.

Now that we understand the use state, we can move on too use effect: [[useEffect]].






