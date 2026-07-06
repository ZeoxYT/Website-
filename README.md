# Zeox.xyz — Website

Lua script encryption & secure hosting platform.

## File Structure (6 files total)

```
zeox-site/
├── index.html                 ← Landing page
├── vercel.json                ← Vercel routing
├── .gitignore
├── README.md
├── website/
│   └── login/
│       └── index.html         ← /website/login  (Google + Email auth)
├── dashboard/
│   └── index.html             ← /dashboard       (Encryptor tool)
└── r/
    └── index.html             ← /r/:id           (Raw script viewer)
```

## Pages & Routes

| URL | File | Description |
|-----|------|-------------|
| `/` | `index.html` | Landing page |
| `/website/login` | `website/login/index.html` | Sign in / Sign up |
| `/dashboard` | `dashboard/index.html` | Script encryptor |
| `/r/SCRIPT_ID` | `r/index.html` | Raw link for each encrypted script |

## Features

### Login page (`/website/login`)
- Google one-click sign-in
- Email + password with email verification
- Password reset via email
- Auto-redirect to `/dashboard` when signed in

### Dashboard (`/dashboard`)
- **Paste** Lua code or **upload** a `.lua` file (drag & drop supported)
- Calls `https://goofyscator.lua.cz/obfuscate` to encrypt
- Replaces first line with `-- This file obfuscator by zeox.xyz BETA [ v1.0 ]`
- **Download** the encrypted `.lua` file
- Saves encrypted script to Firebase Realtime Database
- Generates a unique **raw link** `/r/SCRIPT_ID` for each script
- History of all encrypted scripts with copy/open actions

### Raw link viewer (`/r/:id`)
- Loads the encrypted script from Firebase by ID
- Copy & download buttons
- Watermark line highlighted

## Firebase Setup (required before going live)

1. [Firebase Console → Authentication → Sign-in methods](https://console.firebase.google.com/project/zeoxxyz/authentication/providers)
   - Enable **Email/Password**
   - Enable **Google**

2. [Firebase Console → Authentication → Settings → Authorized domains](https://console.firebase.google.com/project/zeoxxyz/authentication/settings)
   - Add your Vercel preview domain (e.g. `zeox-xyz.vercel.app`)
   - Add your production domain (e.g. `zeox.xyz`)

3. [Firebase Console → Realtime Database → Rules](https://console.firebase.google.com/project/zeoxxyz/database/zeoxxyz-default-rtdb/rules)
   - Set rules to allow authenticated writes and public reads for scripts:
   ```json
   {
     "rules": {
       "scripts": {
         "$id": {
           ".read": true,
           ".write": "auth != null"
         }
       }
     }
   }
   ```

## Deploy to Vercel via GitHub

```bash
# 1 — Push to GitHub
cd zeox-site
git init
git add .
git commit -m "feat: Zeox.xyz — login + encryptor dashboard"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/zeox-site.git
git push -u origin main

# 2 — Import on Vercel
# Go to vercel.com → New Project → select your repo → Deploy
# (No build settings needed — it's a static site)
```

## Deploy via Vercel CLI

```bash
npm i -g vercel
cd zeox-site
vercel --prod
```
