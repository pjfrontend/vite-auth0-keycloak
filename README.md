## 🔐 Vite + React + Auth0

### 🛠️ What You Need

* A free **Auth0 account**: [https://auth0.com](https://auth0.com)
* A React app (or start fresh with `npx create-react-app`)

---

## ⚙️ Step 1: Set Up Auth0

1. **Create an Auth0 account** at [auth0.com](https://auth0.com)

2. Go to the **Applications** tab → Click **“Create Application”**

   * Name it something like `My React App`
   * Choose **“Single Page Web Applications”**
   * Click **Create**

3. In **Settings**, set these values:

   * **Allowed Callback URLs:** `http://localhost:3000`
   * **Allowed Logout URLs:** `http://localhost:3000`
   * **Allowed Web Origins:** `http://localhost:3000`
   * Click **Save Changes**

---

## 📦 Step 2: Install Auth0 React SDK

Inside your React app folder:

```bash
npm install @auth0/auth0-react
```

---

## 🧠 Step 3: Wrap Your App with Auth0Provider

In your `src/index.js` or `src/main.jsx`:

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { Auth0Provider } from "@auth0/auth0-react";

const domain = "YOUR_DOMAIN"; // e.g. dev-abc123.us.auth0.com
const clientId = "YOUR_CLIENT_ID";

ReactDOM.render(
  <Auth0Provider
    domain={domain}
    clientId={clientId}
    authorizationParams={{ redirect_uri: window.location.origin }}
  >
    <App />
  </Auth0Provider>,
  document.getElementById("root")
);
```

➡️ Get the `domain` and `clientId` from your Auth0 App Settings.

---

## 🧪 Step 4: Add Login, Logout, and Profile

In `App.js`, use the Auth0 hooks:

```jsx
import React from "react";
import { useAuth0 } from "@auth0/auth0-react";

function App() {
  const {
    loginWithRedirect,
    logout,
    isAuthenticated,
    user,
    isLoading,
  } = useAuth0();

  if (isLoading) return <div>Loading...</div>;

  return (
    <div style={{ padding: "2rem", fontFamily: "Arial" }}>
      {!isAuthenticated ? (
        <button onClick={() => loginWithRedirect()}>Log In</button>
      ) : (
        <>
          <h2>Welcome, {user.name}!</h2>
          <img src={user.picture} alt="Profile" style={{ borderRadius: "50%" }} />
          <p>Email: {user.email}</p>
          <button onClick={() => logout({ logoutParams: { returnTo: window.location.origin } })}>
            Log Out
          </button>
        </>
      )}
    </div>
  );
}

export default App;
```

---

## ✅ That’s It!

Now when you run:

```bash
npm run dev
```

You'll be able to log in using Auth0 and access user profile info!

---

### ✅ Vite Environment Variable Setup

#### 1. **Create a `.env` file in your project root:**

```env
VITE_AUTH0_DOMAIN=dev-abc123.us.auth0.com
VITE_AUTH0_CLIENT_ID=abc123XYZ456
```

> 🔐 Note: Vite requires all env vars **to start with `VITE_`** to be exposed in your frontend code.

---

#### 2. **Access them in your code like this:**

```js
const domain = import.meta.env.VITE_AUTH0_DOMAIN;
const clientId = import.meta.env.VITE_AUTH0_CLIENT_ID;
```

---

#### 3. **Update your `main.jsx` or wherever you wrap your app:**

```jsx
<Auth0Provider
  domain={import.meta.env.VITE_AUTH0_DOMAIN}
  clientId={import.meta.env.VITE_AUTH0_CLIENT_ID}
  authorizationParams={{ redirect_uri: window.location.origin }}
>
  <App />
</Auth0Provider>
```

---

#### 4. **.gitignore it (optional but recommended):**

```bash
# .gitignore
.env
```

---

### 💡 Pro Tips

* You can also create `.env.development` or `.env.production` for different environments.
* After updating your `.env` file, restart the dev server:

  ```bash
  npm run dev
  ```

---
