# Mengaktifkan login email/Google dan sinkron antar perangkat

1. Buka https://console.firebase.google.com → **Add project** (gratis, paket Spark cukup).
2. **Build → Authentication → Get started**, aktifkan provider **Email/Password** dan **Google**.
3. **Authentication → Settings → Authorized domains** → tambahkan domain hosting kamu (mis. `duitku.web.app`, domain sendiri). `localhost` sudah ada.
4. **Build → Firestore Database → Create database** (mode production, pilih region terdekat, mis. `asia-southeast2`).
5. Tab **Rules**, tempel lalu **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

6. **Project settings → General → Your apps → Web (</>)** → daftarkan app, salin objek `firebaseConfig`, tempel nilainya ke `firebase-config.js`.
7. Upload `index.html`, `style.css`, `script.js`, `firebase-config.js` ke hosting (HTTPS wajib; login Google tidak jalan dari `file://`).

Catatan: tiap bagian data (transaksi, dompet, dst.) disimpan sebagai satu dokumen Firestore dengan batas 1 MB.
Riwayat chat AI, tema, PIN, dan waktu backup terakhir sengaja tidak disinkronkan (khusus perangkat).
