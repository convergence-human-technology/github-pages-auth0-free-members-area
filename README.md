<p align="center">
  <img src="https://github.com/convergence-human-technology/github-pages-auth0-free-members-area/raw/main/fabien-conejero-131313.jpg" alt="github pages auth0 free members area / Convergence" width="100%" height="100%">
</p>

# Github Pages Auth0 - Free Members Aarea

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Pages](https://img.shields.io/badge/Hosting-GitHub%20Pages-blue)](https://pages.github.com/)
[![Auth0](https://img.shields.io/badge/Auth-Auth0-orange)](https://auth0.com/)
[![HTML](https://img.shields.io/badge/Language-HTML%20%2F%20JS-lightgrey)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![No server required](https://img.shields.io/badge/Server-None%20required-brightgreen)]()
[![Free](https://img.shields.io/badge/Cost-100%25%20Free-success)]()

**Author :** Convergence
**License :** MIT
**Date :** March 18, 2026

---

## What this project is about

This repository documents, step by step, how to build a complete website that includes a members-only area, without paying anything, without owning a server, and without writing a single line of backend code.

Everything runs on two free services :

- **GitHub Pages** handles the hosting and gives you a public URL
- **Auth0** handles login, registration, and user management

The result is a real, working website accessible at a permanent URL, protected by a login system, where only registered members can access certain pages.

This guide was written so that a 16-year-old with no technical background can follow it from start to finish and succeed on the first try.

---

## Repository name and keywords

**Repository name :** `github-pages-auth0-free-members-area`

**Topics / keywords :** `github-pages` `auth0` `authentication` `members-area` `static-site` `free-hosting` `javascript` `html` `tutorial` `beginner` `no-server` `spa` `oauth2` `implicit-flow`

**Short description :** Build a free website with a members-only area using GitHub Pages for hosting and Auth0 for authentication. No server, no cost, step-by-step guide for complete beginners.

---

## How this guide is structured

| Section | Content |
|---|---|
| 1 | Understanding what GitHub Pages can and cannot do |
| 2 | Creating the repository and activating GitHub Pages |
| 3 | The file structure you need |
| 4 | Creating an Auth0 account and application |
| 5 | Writing the three HTML files |
| 6 | Configuring Auth0 settings |
| 7 | Deploying and testing |
| 8 | Problems we encountered and how we solved them |
| 9 | What to do if the button still does not work |
| 10 | Alternative authentication services |

---

## Section 1 : Understanding what GitHub Pages can and cannot do

GitHub Pages is a free service included with every GitHub account. It hosts files directly from a repository and makes them available at a public URL.

**What it supports :**
- HTML files
- CSS files
- JavaScript files
- Images and other static assets
- HTTPS by default, at no cost

**What it does not support :**
- PHP
- Node.js
- Python
- Any server-side code
- Databases

This means you cannot build a login system using only GitHub Pages. The login logic must come from an external service. That is exactly what Auth0 provides.

---

## Section 2 : Creating the repository and activating GitHub Pages

### Step 1 : Create the repository

1. Go to github.com and sign in
2. Click the green button "New" to create a repository
3. Choose a name, for example `site`
4. Set visibility to **Public** (required for free GitHub Pages)
5. Check "Add a README file"
6. Click "Create repository"

### Step 2 : Activate GitHub Pages

1. Inside your repository, click the "Settings" tab
2. In the left menu, click "Pages"
3. Under "Source", select "Deploy from a branch"
4. Under "Branch", click the dropdown and select your main branch (it may appear as "main" or "principal" depending on your language settings)
5. Leave the folder set to "/ (root)"
6. Click "Save"

After a minute or two, GitHub will display a green banner at the top of the Pages settings page with your site URL. It will look like this :

```
https://your-username.github.io/site/
```

### Important note about branch names

GitHub may display branch names in your own language. In French, the main branch appears as "principal". In English it appears as "main". Both refer to the same thing. Select whichever appears in your dropdown.

---

## Section 3 : The file structure you need

For a site with a members area using Auth0, you need exactly three HTML files at the root of your repository.

```
your-repository/
├── index.html        (public homepage with login button)
├── callback.html     (Auth0 redirect handler, required)
├── membres.html      (private page, only visible after login)
└── README.md
```

You do not need a separate JavaScript file. All the logic goes inside each HTML file using inline script tags.

**Why three files and not one ?**

Auth0 works by sending the user to its own login page, hosted on Auth0 servers. After the user logs in, Auth0 redirects the browser back to a URL you specify. That URL must be a real page in your site. That is what `callback.html` is for. It receives the login result, stores it, and immediately redirects the user to the members page.

---

## Section 4 : Creating an Auth0 account and application

### Step 1 : Sign up for Auth0

1. Go to auth0.com
2. Click "Sign Up"
3. Choose "Continue with GitHub" to avoid creating another password
4. GitHub will ask you to authorize Auth0, click "Authorize"

### Step 2 : The setup wizard

Auth0 will ask you a few questions during first setup.

**Role question :** Select "Yes, coding" since you will be writing HTML and JavaScript.

**Region question :** Check the box "I need advanced parameters". A region selector will appear. Select "EU" (Europe). This is important if you are located in France or anywhere in Europe, for GDPR compliance. By default Auth0 assigns the United States region, which is not correct for European users.

**Tenant name :** You will be asked to name your tenant. This becomes part of your Auth0 domain. Choose something short and memorable, for example `convergence-tech`. The result will be `convergence-tech.eu.auth0.com`. You cannot change this name later.

### Step 3 : Create an application

After setup, you arrive at the Auth0 dashboard.

1. Click "Applications" in the left menu
2. Click "Create Application"
3. Name it something clear, for example "Convergence Site"
4. Select the type **"Single Page Application"**
5. Click "Create"

Auth0 will show you a quickstart page. You do not need to follow the quickstart. Instead, click the **"Settings"** tab at the top.

### Step 4 : Note your credentials

In the Settings tab, you will see two values you need to copy :

| Field | Example value |
|---|---|
| Domain | `convergence-tech.eu.auth0.com` |
| Client ID | `ONtxpyovHGewrl4669n3Qz8RJYot27AS` |

**Critical warning about the Client ID :** Copy this value using the clipboard icon next to it. Do not type it by hand. The characters `0` (zero) and `O` (letter O), and `1` (one) and `l` (lowercase L) look almost identical in most fonts. Typing the wrong character will cause Auth0 to return an "Unknown client" error. This was the most common mistake during development of this project.

### Step 5 : Configure the allowed URLs

Still on the Settings tab, scroll down to the section called "Application URIs". Fill in these three fields :

**Allowed Callback URLs :**
```
https://your-username.github.io/site/callback.html
```

**Allowed Logout URLs :**
```
https://your-username.github.io/site/index.html
```

**Allowed Web Origins :**
```
https://your-username.github.io
```

Replace `your-username` with your actual GitHub username.

Scroll to the bottom and click **"Save"**.

### Step 6 : Enable the Implicit grant type

This project uses the OAuth2 implicit flow to authenticate users. On some Auth0 accounts this is not enabled by default.

1. On the Settings tab, scroll to the very bottom
2. Click the "Advanced Settings" section to expand it
3. Click the "Grant Types" tab
4. Make sure "Implicit" is checked
5. Click "Save"

If you skip this step, Auth0 will display an "Oops, something went wrong" error when the user clicks the login button.

---

## Section 5 : The three HTML files

### File 1 : index.html

This is your public homepage. It shows a login button. After login, the button changes to "Members Area" and "Logout".

Replace `YOUR-DOMAIN` and `YOUR-CLIENT-ID` with your actual values from Auth0.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Convergence - Accueil</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 800px; margin: 50px auto; text-align: center; }
    button { padding: 10px 20px; margin: 10px; cursor: pointer; border-radius: 5px; border: none; font-size: 16px; color: white; }
    #btn-login { background: #635DFF; }
    #btn-membres { background: #28a745; }
    #btn-logout { background: #dc3545; }
  </style>
</head>
<body>
  <h1>Convergence Humaine et Technologique</h1>
  <p>Bienvenue sur notre site.</p>
  <div id="auth-buttons">
    <button id="btn-login">Se connecter</button>
    <button id="btn-membres" style="display:none">Espace Membres</button>
    <button id="btn-logout" style="display:none">Deconnexion</button>
  </div>

  <script>
    const AUTH0_DOMAIN = "YOUR-DOMAIN";
    const CLIENT_ID = "YOUR-CLIENT-ID";
    const REDIRECT_URI = "https://YOUR-USERNAME.github.io/site/callback.html";
    const BASE_URL = "https://YOUR-USERNAME.github.io/site";

    const token = sessionStorage.getItem("auth_token");
    if (token) {
      document.getElementById("btn-login").style.display = "none";
      document.getElementById("btn-membres").style.display = "inline";
      document.getElementById("btn-logout").style.display = "inline";
    }

    document.getElementById("btn-login").onclick = () => {
      const state = Math.random().toString(36).substring(2);
      sessionStorage.setItem("auth_state", state);
      const url = `https://${AUTH0_DOMAIN}/authorize?response_type=token&client_id=${CLIENT_ID}&redirect_uri=${encodeURIComponent(REDIRECT_URI)}&scope=openid%20profile%20email&state=${state}`;
      window.location.href = url;
    };

    document.getElementById("btn-membres").onclick = () => {
      window.location.href = BASE_URL + "/membres.html";
    };

    document.getElementById("btn-logout").onclick = () => {
      sessionStorage.clear();
      window.location.href = `https://${AUTH0_DOMAIN}/v2/logout?returnTo=${encodeURIComponent(BASE_URL + "/index.html")}&client_id=${CLIENT_ID}`;
    };
  </script>
</body>
</html>
```

### File 2 : callback.html

This file handles the return from Auth0 after login. The user never sees it for more than a fraction of a second. It reads the token from the URL, stores it, and redirects to the members page.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Connexion en cours...</title>
</head>
<body>
  <p>Connexion en cours...</p>
  <script>
    const hash = window.location.hash.substring(1);
    const params = new URLSearchParams(hash);
    const token = params.get("access_token");

    if (token) {
      sessionStorage.setItem("auth_token", token);
      window.location.replace("https://YOUR-USERNAME.github.io/site/membres.html");
    } else {
      window.location.replace("https://YOUR-USERNAME.github.io/site/index.html");
    }
  </script>
</body>
</html>
```

### File 3 : membres.html

This is the protected page. If no token is found in session storage, the user is sent back to the homepage immediately. If a token exists, it calls the Auth0 userinfo endpoint to get the user's name, email and profile picture.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Espace Membres - Convergence</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 800px; margin: 50px auto; text-align: center; }
    #user-card { background: #f4f4f4; border-radius: 10px; padding: 30px; margin: 20px auto; }
    button { padding: 10px 20px; background: #dc3545; color: white; border: none; border-radius: 5px; cursor: pointer; font-size: 16px; }
  </style>
</head>
<body>
  <h1>Espace Membres</h1>
  <div id="user-card">Chargement...</div>
  <button id="btn-logout">Deconnexion</button>

  <script>
    const AUTH0_DOMAIN = "YOUR-DOMAIN";
    const CLIENT_ID = "YOUR-CLIENT-ID";
    const BASE_URL = "https://YOUR-USERNAME.github.io/site";

    const token = sessionStorage.getItem("auth_token");
    if (!token) {
      window.location.replace(BASE_URL + "/index.html");
    } else {
      fetch(`https://${AUTH0_DOMAIN}/userinfo`, {
        headers: { Authorization: `Bearer ${token}` }
      })
      .then(r => r.json())
      .then(user => {
        document.getElementById("user-card").innerHTML = `
          <img src="${user.picture}" width="80" style="border-radius:50%"><br>
          <h2>Bonjour ${user.name}</h2>
          <p>${user.email}</p>
          <p>Vous etes connecte a l espace membres.</p>
        `;
      })
      .catch(() => {
        document.getElementById("user-card").innerHTML = "<p>Vous etes connecte.</p>";
      });
    }

    document.getElementById("btn-logout").onclick = () => {
      sessionStorage.clear();
      window.location.href = `https://${AUTH0_DOMAIN}/v2/logout?returnTo=${encodeURIComponent(BASE_URL + "/index.html")}&client_id=${CLIENT_ID}`;
    };
  </script>
</body>
</html>
```

---

## Section 6 : Deploying and testing

### How to create files on GitHub without a code editor

You do not need to install anything on your computer. GitHub lets you create and edit files directly in the browser.

1. Go to your repository on github.com
2. Click "Add file" then "Create new file"
3. Type the filename in the name field at the top
4. Paste the HTML code into the text area
5. Scroll down and click "Commit changes"
6. In the dialog that appears, select "Commit directly to the main branch"
7. Click "Commit changes"

Repeat this for each of the three files.

### How to edit an existing file

1. Click on the filename in your repository
2. Click the pencil icon in the top right of the file view
3. Make your changes
4. Scroll down and commit

### Checking deployment status

After each commit, GitHub Pages takes about one to two minutes to rebuild your site. You can check the status :

1. Go to your repository
2. Look at the right side panel under "Deployments"
3. A green dot means the site is live
4. An orange dot means it is still building
5. A red dot means there was an error

### Testing the full flow

1. Open your site URL in a browser
2. You should see the homepage with a login button
3. Click the login button
4. You should be redirected to Auth0's login page, which shows your tenant name and your app name
5. Create an account using email or Google
6. After login, Auth0 redirects to callback.html, which immediately redirects to membres.html
7. You should see your name, email and profile picture

---

## Section 7 : Verifying everything is correct

Use this checklist before testing :

| Item to verify | Where to check |
|---|---|
| Repository is Public | Repository Settings, top of General tab |
| GitHub Pages is enabled on main branch | Repository Settings, Pages section |
| index.html exists at root | Repository file list |
| callback.html exists at root | Repository file list |
| membres.html exists at root | Repository file list |
| Client ID in HTML files matches Auth0 exactly | Compare character by character |
| Domain in HTML files matches Auth0 exactly | Settings tab in Auth0 |
| Callback URL in Auth0 matches your actual GitHub Pages URL | Auth0 Application Settings |
| Logout URL in Auth0 matches your actual GitHub Pages URL | Auth0 Application Settings |
| Web Origins in Auth0 contains your GitHub Pages domain | Auth0 Application Settings |
| Implicit grant type is enabled | Auth0 Advanced Settings, Grant Types tab |

---

## Section 8 : Problems encountered and solutions found

This section documents every real problem that appeared during the development of this project, and exactly how each one was solved. Nothing here is theoretical.

### Problem 1 : The login button does nothing

**Symptom :** Clicking the button has no visible effect.

**Cause :** The Auth0 SDK JavaScript file from the CDN is blocked before it loads. This makes `createAuth0Client` undefined.

**Solution :** Stop using the Auth0 SDK entirely. Use direct OAuth2 URL construction instead. The code in Section 5 of this guide uses this approach. It requires no external JavaScript file and works in all browsers.

### Problem 2 : Console error "createAuth0Client is not defined"

**Symptom :** Opening browser developer tools (F12) and clicking the Console tab shows a red error : `ReferenceError: createAuth0Client is not defined`.

**Cause :** The Auth0 SPA SDK script tag is either blocked, failing to load, or using an incompatible version.

**Solution :** Remove all script tags that load the Auth0 SDK. Use only inline JavaScript with direct OAuth2 URLs as shown in the code above. The CDN at `cdn.auth0.com` returns "Access Denied" in some network configurations, including when Avast antivirus Web Shield is active.

### Problem 3 : Auth0 error page "Oops, something went wrong" with "Unknown client"

**Symptom :** Clicking the login button redirects to Auth0, but Auth0 shows an error page with the message `invalid_request: Unknown client: [your client id]`.

**Cause A :** The Client ID in your HTML file contains a typo. This is the most common cause. The characters `0` and `O`, or `1` and `l`, were confused when typing.

**Solution A :** Go to Auth0 Dashboard, click Applications, and use the clipboard copy icon next to the Client ID. Paste it directly into your HTML file. Do not type it manually.

**Cause B :** The Implicit grant type is not enabled on your Auth0 application.

**Solution B :** Go to Auth0 Dashboard, click your application, go to Settings, scroll to Advanced Settings, open the Grant Types tab, check "Implicit", and save.

### Problem 4 : CDN file cannot be downloaded

**Symptom :** Trying to download the Auth0 SDK file from `cdn.auth0.com` returns "Access Denied" in the browser, or the download is blocked by antivirus software.

**Solution :** Do not use the Auth0 SDK at all. The code in this guide uses no external JavaScript libraries. Every feature is implemented with standard browser APIs and direct HTTP requests.

### Problem 5 : GitHub Pages shows an old version of the site

**Symptom :** You committed changes but the live site still shows the old version.

**Solution :** GitHub Pages caches aggressively. After committing, wait two minutes, then press Ctrl+Shift+R (or Cmd+Shift+R on Mac) to force a hard reload that bypasses the browser cache. You can also verify the deployment was successful by checking the Deployments panel in your repository.

### Problem 6 : Auth0 is assigned to the United States region instead of Europe

**Symptom :** During Auth0 account creation, the domain ends in `.us.auth0.com` instead of `.eu.auth0.com`.

**Cause :** Auth0 defaults to the US region. If you did not check "I need advanced parameters" during setup, you cannot change the region after the tenant is created.

**Solution :** If your tenant is already created with the wrong region and GDPR compliance matters to you, create a new tenant. In the Auth0 dashboard, click your tenant name at the top left, then click "Create a tenant", and this time select the EU region.

### Problem 7 : The members page shows "Loading..." but never displays user info

**Symptom :** After login, the members page appears but the user card shows "Chargement..." and never changes.

**Cause :** The fetch request to the Auth0 userinfo endpoint is failing silently. This can happen if the access token is expired or if there is a CORS issue.

**Solution :** The `.catch()` block in the code handles this gracefully. The user will see "Vous etes connecte" instead of their profile. This is acceptable behavior. If you want to debug further, open F12, go to the Network tab, reload the page, and look for a request to `userinfo`. Check its status code and response.

---

## Section 9 : Choosing between Auth0, Firebase, and Supabase

All three services offer a free tier that is more than enough to start a project. Here is a comparison to help you decide.

### Free tier limits

| Service | Free active users per month | Notes |
|---|---|---|
| Auth0 | 25,000 | Generous free tier, specialized in authentication |
| Firebase | 50,000 | Google product, integrated with broader ecosystem |
| Supabase | 50,000 | Open source alternative, includes database |

### When the free tier ends

| Service | First paid tier | Cost |
|---|---|---|
| Auth0 | 500 users | 150 USD per month |
| Firebase | Pay as you go | Around 0.003 USD per user above free limit |
| Supabase | Pro plan | 25 USD per month, includes 100,000 users |

### Advanced features and their costs

Some features are only available on paid plans, even with few users.

| Feature | Auth0 | Firebase | Supabase |
|---|---|---|---|
| SAML / SSO | From 800 USD/month | From 0.015 USD per active user | From 599 USD/month |
| SMS authentication | Not included in free tier | 0.01 to 0.06 USD per SMS | Not included in free tier |
| Custom domain for login page | Requires credit card validation | Not available on free tier | Not available on free tier |

### Which one to choose for this project

If you are starting and want the simplest setup with the best documentation, use **Auth0**. This guide was built using Auth0.

If you expect rapid growth and want to avoid surprise bills, use **Firebase** or **Supabase**. Both offer more free users and more predictable pricing.

For a personal project, a school project, or a small community site, any of the three free tiers will last for a long time before you need to think about paying anything.

---

## Section 10 : How the full authentication flow works

Understanding this flow helps you debug issues when they appear.

```
User clicks "Login" on index.html
        |
        v
Browser redirects to Auth0 login page
(hosted at convergence-tech.eu.auth0.com)
        |
        v
User enters email and password, or clicks "Continue with Google"
        |
        v
Auth0 verifies credentials
        |
        v
Auth0 redirects browser to callback.html
with an access token in the URL fragment (#access_token=...)
        |
        v
callback.html reads the token from the URL
stores it in sessionStorage
redirects to membres.html
        |
        v
membres.html checks sessionStorage for the token
if no token found, sends user back to index.html
if token found, calls Auth0 userinfo endpoint
displays the user's name, email and picture
```

**Why sessionStorage and not localStorage ?**

`sessionStorage` is cleared automatically when the browser tab is closed. This means users are logged out when they close the tab. `localStorage` would keep them logged in permanently. For a members area, `sessionStorage` is the safer default.

---

## Repository structure

```
github-pages-auth0-free-members-area/
├── .github/
│   ├── workflows/
│   │   └── ci.yml
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/
│   └── architecture.md
├── src/
│   ├── main/
│   └── test/
├── scripts/
├── img/
├── .gitignore
├── .editorconfig
├── LICENSE
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── SECURITY.md
└── CODE_OF_CONDUCT.md
```

---

## License

MIT License

Copyright (c) 2026 Convergence

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.

---

## Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you would like to change.

---

*Convergence - March 18, 2026*
