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

...