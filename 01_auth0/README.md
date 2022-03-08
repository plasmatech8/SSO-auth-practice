# Auth0 Integration Practice

Following:
* Auth0 getting started instructions with a new SPA app
* [Authenticating Svelte Applications](https://auth0.com/blog/authenticating-svelte-apps/)

Contents:
- [Auth0 Integration Practice](#auth0-integration-practice)
  - [01. Init](#01-init)
  - [02. Configure Auth0](#02-configure-auth0)
  - [03. Install SDK](#03-install-sdk)
  - [04. SDK Usage](#04-sdk-usage)
  - [05. SDK Implementation](#05-sdk-implementation)

## 01. Init

```
npm create vite@latest
cd vite-app
npm install
```

## 02. Configure Auth0

1. Go to Application Settings
2. Go to your application (or create one)
3. Copy the "Client ID" and "Domain"
4. Configure "Allowed Callback URLs" to `https://localhost3000` (required for log-in)
5. Configure "Allowed Logout URLs" to `https://localhost:3000` (required for log-out)
6. Configure "Allowed Web Origins" to `https://localhost:3000` (required for refreshing tokens)

## 03. Install SDK

```bash
npm install @auth0/auth0-spa-js
```

## 04. SDK Usage

```js
let client = await createAuth0Client({
    domain: config.domain,
    client_id: config.clientId
});
// ...
await client.loginWithPopup(options);
// ...
await client.logout();

```

## 05. SDK Implementation

Under `auth_config.js` we have our config object, which contains the "Domain" and "Client ID"
taken from the Auth0 console.

Under `authService.js` we have helper methods used to log in:
```js
async function createClient() {
    let auth0Client = await createAuth0Client({
        domain: config.domain,
        client_id: config.clientId
    });
    return auth0Client;
}

async function loginWithPopup(client, options) {
    popupOpen.set(true);
    try {
        await client.loginWithPopup(options);
        user.set(await client.getUser());
        isAuthenticated.set(true);
    } catch (e) {
        console.error(e);
    } finally {
        popupOpen.set(false);
    }
}

function logout(client) {
  return client.logout();
}
```

In `App.svelte` we have methods:
```js
onMount(async () => {
    auth0Client = await auth.createClient();
    isAuthenticated.set(await auth0Client.isAuthenticated());
    user.set(await auth0Client.getUser());
    loading = false;
});

function login() {
    auth.loginWithPopup(auth0Client);
}

function logout() {
    auth.logout(auth0Client);
}
```

OnMount will check if the user is currently authenticated and update the global state/store.
Note that we cannot simply use `login` because the user may already be logged in and does not need a dialog.
Plus, the user may not want to be logged in automatically.
