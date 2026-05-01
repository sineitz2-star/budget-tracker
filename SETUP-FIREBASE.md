# Setup Firebase untuk Budget Tracker dengan Login

## File yang digunakan
- `index-with-firebase.html` - Versi dengan login & cloud storage

## Langkah Setup

### 1️⃣ Buat Firebase Project
- Buka https://console.firebase.google.com
- Klik "Create Project" 
- Nama: `budget-tracker`
- Klik "Next" → Disable Google Analytics → "Create project"
- Tunggu selesai

### 2️⃣ Setup Authentication (Email/Password)
- Sidebar kiri → **Authentication**
- Klik **"Get Started"**
- Pilih **Email/Password** (pilihan pertama)
- Toggle **Enabled** untuk Email/Password
- Klik **Save**

### 3️⃣ Setup Firestore Database
- Sidebar kiri → **Firestore Database**
- Klik **"Create Database"**
- Pilih lokasi: **asia-southeast1** (atau terdekat)
- Pilih **"Start in production mode"**
- Klik **"Create"**
- Tunggu database dibuat

### 4️⃣ Update Firestore Security Rules
- Di database, buka tab **"Rules"**
- Hapus semua rules yang ada, replace dengan:

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

- Klik **"Publish"**

### 5️⃣ Dapatkan Firebase Config
- Klik **Settings** (gear icon) di atas "Project Overview"
- Pilih **"Project settings"**
- Scroll ke bagian **"Your apps"**
- Jika belum ada, klik **"</>  Add app"** → pilih **Web**
- Nama: `budget-tracker`
- Copy seluruh `firebaseConfig` object
- Contoh:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDxxxxxxxxxxxxxxxxxxxxxx",
  authDomain: "budget-tracker-xxxxx.firebaseapp.com",
  projectId: "budget-tracker-xxxxx",
  storageBucket: "budget-tracker-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdefg"
};
```

### 6️⃣ Update index-with-firebase.html
- Buka file `index-with-firebase.html`
- Cari bagian: `// ============ FIREBASE CONFIG ============`
- Ganti `firebaseConfig` dengan config dari Firebase:
```javascript
const firebaseConfig = {
  // Paste config di sini
};
```

### 7️⃣ Deploy atau Test Lokal
**Opsi A: Test di lokal (dev)**
- Start local server: `python -m http.server 8000`
- Buka: `http://localhost:8000/index-with-firebase.html`

**Opsi B: Deploy ke GitHub Pages**
- Replace `index.html` dengan `index-with-firebase.html` 
- Rename: `mv index-with-firebase.html index.html`
- Push ke GitHub
- GitHub akan auto-deploy

## Fitur
✅ Login/Register dengan email & password
✅ Data tersimpan di cloud (Firebase)
✅ Bisa akses dari device lain
✅ Auto-sync data
✅ Logout & Privacy

## Troubleshooting

**Error: "Firebase config not found"**
- Pastikan sudah isi `firebaseConfig` dengan benar di kode

**Error: "Unauthorized"**
- Check Firestore Rules (step 4)
- Pastikan user terdaftar

**Error: "Project does not exist"**
- Ganti `projectId` sesuai project Firebase Anda
