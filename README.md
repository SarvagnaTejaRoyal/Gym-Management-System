# Gym Management System

This is a web-based Gym Management System using **Firebase Authentication + Firestore + Realtime Database**.
---

## KEY Features

* Firebase Authentication (Admin & Member login)
* Role-based access (admin / member)
* Admin dashboard (add members, notifications)
* Member dashboard (view dues)
* Diet plans & supplements

---

## Default Credentials

* **Admin**

  * Email: admin@gmail.com
  * Password: admin123

* **Member**

  * Email: member@gmail.com
  * Password: member123
---

# ⚙️ Firebase Setup (VERY IMPORTANT)

Follow these steps correctly to avoid login errors.
---

## 1️ Enable Authentication (Email/Password)

1. Go to **Firebase Console**
2. Open your project
3. Go to:
   ```
   Authentication → Sign-in method
   ```
4. Enable:
   ✔ Email/Password
---

## 2️ Create Users in Authentication

Go to:
```
Authentication → Users → Add User
```

Create:

### Admin User :
* Email: [admin@gmail.com](mailto:admin@gmail.com)
* Password: admin123

### Member User :
* Email: [member@gmail.com](mailto:member@gmail.com)
* Password: member123

IMPORTANT:
After creating users, copy their **User UID**.
---

## 3️ Create Firestore Database

Go to:

```
Firestore Database → Create Database
```
* Start in **Test mode (for development)** OR
* Use rules provided below
---

## 4️ Create `users` Collection

Go to:
```
Firestore → Start Collection
```

Collection name:
```
users
```

## 5️ Create Documents with Correct UID

⚠️ THIS IS THE MOST IMPORTANT STEP

For each user:

### Admin Document

* Click **Add Document**
* Document ID = **Admin UID (from Authentication)**
* Add field:

| Field | Type   | Value |
| ----- | ------ | ----- |
| role  | string | admin |

---

### Member Document

* Click **Add Document**
* Document ID = **Member UID (from Authentication)**
* Add field:

| Field | Type   | Value  |
| ----- | ------ | ------ |
| role  | string | member |

---

### Final Structure

```
users
   admin_uid_from_auth
      role: "admin"

   member_uid_from_auth
      role: "member"
```

## 6️ Firestore Security Rules

Go to:
```
Firestore → Rules
```
Replace with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null && request.auth.uid == userId;
      allow write: if request.auth != null && request.auth.uid == userId;
    }

  }
}
```
Click Publish
---

## 7️ Firebase Configuration (firebase-config.js)

Where to get it:

1.Go to Firebase Console
2.Open your project
3.Click ⚙️ Project Settings
4.Scroll to Your Apps
5.Click Web App (</>)
6.Copy the config

Add in firebase-config.js
```
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "YOUR_DATABASE_URL",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "XXXXXXX",
  appId: "XXXXXXX",
  measurementId: "XXXXXXX"
};
firebase.initializeApp(firebaseConfig);
 ```
 Note:
```This project uses Firebase v8 (CDN version)```

So use:
```firebase.initializeApp(firebaseConfig);```

NOT:
```initializeApp(firebaseConfig); ```

---
## Common Errors & Fixes

### ❌ Error: "Missing or insufficient permissions"

✔ Fix:

* Check Firestore rules
* Make sure user is logged in

---

### ❌ Error: "No role found for this user"

✔ Fix:

* Ensure document exists in `users` collection
* Document ID must equal **User UID**

---

### ❌ Login not redirecting

✔ Fix:

* Check role value:

  * must be exactly `admin` or `member`
  * no uppercase

---

### ❌ Wrong Document IDs

❌ WRONG:

```
users
   random123
```

✔ CORRECT:

```
users
   actual_user_uid
```

---

## Important Concept

Your login works like this:

1. User logs in using Firebase Auth
2. App gets `user.uid`
3. App reads:

   ```
   users/{uid}
   ```
4. Checks role
5. Redirects accordingly

So **UID must match Firestore document ID**

---

## How to Run

1. Download project
2. Update `firebase-config.js`
3. Open `index.html`
4. Login using credentials

---

## Developed By

Sarvagna Teja
