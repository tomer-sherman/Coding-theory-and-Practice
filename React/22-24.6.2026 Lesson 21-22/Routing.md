

````
# 🧭 React Router Setup Guide

A complete, step-by-step guide to setting up local routing in a React (TypeScript) application.

---

## ⚙️ System Requirements

Before starting, you need to install the correct NPM packages. Run these commands in your terminal:

```bash
# 1. Install the main router package (Required for production)
npm install react-router-dom

# 2. Install the TypeScript types (Development only)
npm install @types/react-router-dom -D
````

## 🚀 Implementation Steps

### Step 1: Wrap Your App in `main.tsx`

After installing the packages, you need to enable routing for your entire app. Go to your `main.tsx` (or `index.tsx`) file and wrap your main `Layout` or `App` component with the `<BrowserRouter>` tag.

TypeScript

```
import { BrowserRouter } from "react-router-dom";
import { Layout } from "./components/Layout";

// Inside your root render:
<BrowserRouter>
    <Layout/>
</BrowserRouter>
```

### Step 2: Create the `Routing` Component

Create a new functional component just for your routes. Inside it, use the `<Routes>` tag to surround each individual `<Route />`.

Every `<Route />` needs two main attributes:

- **`path`**: The URL path in the browser's address bar (e.g., `/page-name`).
    
- **`element`**: The actual React component that will render when that path is visited.
    



```
import { Routes, Route, Navigate } from "react-router-dom";
import { Ytvideo } from "../pages/ytvideo/ytvideo";
import { About } from "../pages/about/about";
import { Products } from "../pages/products/products";
import { Story } from "../pages/story/story";

export function Routing() {
    return (
        <Routes>
            {/* Default route: Redirects users immediately when they enter the site */}
            <Route element="{<Navigate" path="/" to="/home"/>} />

            {/* Individual pages */}
            <Route element="{<Ytvideo" path="/home"/>} />
            <Route element="{<Products" path="/products"/>} />
            <Route element="{<Story" path="/story"/>} />
            <Route element="{<About" path="/about"/>} />
        </Routes>
    );
}
```

### Step 3: Place the Router in Your `Layout`

Go to your `Layout` component and place your new `<Routing />` component exactly where you want the dynamic page content to appear. This is usually placed inside the `<main>` HTML tag.



```
import { Routing } from "./Routing";
import { Menu } from "./Menu";

export function Layout() {
    return (
        <div className="layout-container">
            <header>
                {/* Navigation goes here! */}
                <Menu/> 
            </header>

            <main>
                {/* 👇 HERE YOU PLACE THE ROUTING COMPONENT 👇 */}
                <Routing/>
            </main>
        </div>
    );
}
```

### Step 4: Create a Navigation Menu

We need a way to navigate between these routes without refreshing the page! Go to your navigation menu component and use the `<NavLink>` or `<Link>` tags provided by React Router.



```
import { NavLink } from "react-router-dom";

export function Menu() {
    return (
        <nav>
            {/* Use the 'to' attribute to match the paths defined in Step 2 */}
            <NavLink to="/home">Home</NavLink>
            <NavLink to="/products">Products</NavLink>
            <NavLink to="/story">Story</NavLink>
            <NavLink to="/about">About</NavLink>
        </nav>
    );
}
```

## 🧠 Code Flow Overview

1. **📦 Download:** Install the correct NPM packages (`react-router-dom` and its `@types`).
    
2. **🌐 Setup:** In `main.tsx`, import `<BrowserRouter>` and wrap your `<Layout />` component with it.
    
3. **🗺️ Map Routes:** Create a `Routing` component. Surround all individual routes with `<Routes>`. Add `path` and `element` attributes to each `<Route />`.
    
4. **🧩 Position:** Place the `<Routing />` component inside your `<Layout />` (usually surrounded by the `<main>` tag).
    
5. **🖱️ Navigate:** Create a clickable navigation menu using `<NavLink to="/some-page">` to direct users to the different routes.
    

> **💡 Note:** If you want to add more pages that need to be routable in the future, simply repeat **Step 2** (add the route) and **Step 4** (add the link)! Happy routing!