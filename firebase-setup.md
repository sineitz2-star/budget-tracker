# Firebase Setup untuk Budget Tracker

## Langkah 1: Buat Firebase Project
1. Buka https://console.firebase.google.com
2. Klik "Create Project" → nama "budget-tracker"
3. Disable Google Analytics → Create Project
4. Tunggu selesai

## Langkah 2: Setup Authentication
1. Di sidebar, klik "Authentication"
2. Klik "Get Started" → pilih "Email/Password"
3. Aktifkan Email/Password → Save

## Langkah 3: Setup Firestore Database
1. Di sidebar, klik "Firestore Database"
2. Klik "Create Database"
3. Pilih lokasi terdekat
4. Pilih "Start in production mode" → Create Database
5. Di Rules tab, ganti dengan rules:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/data/{document=**} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
```

## Langkah 4: Dapatkan Config
1. Klik Settings (gear icon) di sidebar
2. Pilih "Project settings"
3. Scroll ke "Your apps" → tambah app (web icon)
4. Nama app: "budget-tracker"
5. Copy firebaseConfig dari script tag
6. Paste di dalam kode HTML di tag CONFIG

## Langkah 5: Paste Config ke index.html
Cari `// FIREBASE CONFIG HERE` dan paste config-nya
