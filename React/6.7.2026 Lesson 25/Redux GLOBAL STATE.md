# 🔄 Redux Global State Management: How It Works

## 🤔 Do You Need Redux?
Before diving into global state management, ask yourself one simple question:

> **Does my app have information that needs to be displayed across multiple, different components?**

It's a boolean question:
* 🔴 **No:** You don't need Redux. Local state is perfectly fine!
* 🟢 **Yes:** You have come to the right place. 

Before diving into Redux, depending on the kind of app you want to create, you also need to understand how **CRUD** (Create, Read, Update, Delete) works and how to manage it.

---

## 🌍 Global State Management Overview
In vanilla JavaScript, you could just throw your global variables into the main script and it would work. However, in **Component-Based Frameworks** (like React), you cannot do this. 

Because components render dynamically, you need a structured method to load and share global data across your app. That is exactly why understanding how to manage local vs. global state is crucial.

---

## 🏗️ Structure Overview
* 📦 **The Store:** The central hub containing all your global variables. It is made up of "slices" because the data is most often arrays of different information.
* 🗺️ **AppState:** Defines the different types of information and data structures the store works with.
* 🍕 **Slices:** Individual segments within the store. Each slice represents a specific category of data managed in the global state.
* ⚙️ **Reducers:** Functions within a specific slice that manage its data. The main actions are typically Adding, Updating, Removing, and Initializing.
* 🚀 **Dispatch:** A function called within a component to trigger an Action.
* 🎬 **Action:** An object containing two properties. The `type` identifies the specific slice you want to modify, and the `payload` holds the actual data you wish to inject into the slice.

---

## 🌊 Data Flow Overview
1. **Trigger:** A `dispatch` function is called, sending an Action to the store.
2. **Targeting:** The Action enters the store. Based on its `type`, it triggers a specific Reducer within a specific Slice.
3. **Updating:** The Reducer updates, deletes, adds, or initializes the data. A *new state* is formed and returned. The global state now contains this newly updated slice.
4. **Re-rendering (Subscription):** If a component is registered to specific slice data using `useSelector`, it will automatically re-render with the newly updated data. 

> 💡 **Summary:** This cycle of re-rendering happens every time the specific slice data that a component is subscribed to gets updated. That perfectly encapsulates the entire Redux code flow!