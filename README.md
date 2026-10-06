# Activity Planner

Frontend statis untuk GitHub Pages dan backend Google Apps Script untuk spreadsheet **Activity Planner Database**.

Repository deployment: `rg-sulselraya/FU-Planner`  
Target path: `https://rgsulselraya.xyz/activityplanner`  
Apps Script API: `https://script.google.com/macros/s/AKfycby8YzNi2tz5t26vJfKiUx12SM9b3qBUMmEQItkTSprTgAPGjzBaYKWgwWQ1H7ObRPl4/exec`

## Struktur

```text
followup-planner/
├── index.html
├── style.css
├── app.js
├── README.md
└── apps-script/Code.gs
```

## Deployment Apps Script

1. Buka spreadsheet dengan ID `1y_FMz0BoNmDpzm32R84I7cIt47DIAeCxMn2TSXPopIA`, lalu Extensions → Apps Script.
2. Buat project baru (atau buka project backend baru), salin isi `apps-script/Code.gs` ke editor, lalu Save.
3. Jalankan `setupAdminPassword()` satu kali. Ganti placeholder `CHANGE_ME_BEFORE_DEPLOY` di fungsi tersebut dengan password admin pilihan Anda sebelum menjalankan.
4. Deploy → New deployment → Web app.
   - Execute as: Me
   - Who has access: Anyone
5. URL deployment yang sedang dikonfigurasi pada frontend: `https://script.google.com/macros/s/AKfycby8YzNi2tz5t26vJfKiUx12SM9b3qBUMmEQItkTSprTgAPGjzBaYKWgwWQ1H7ObRPl4/exec`. Jangan masukkan password, PIN, credential, atau Spreadsheet ID ke frontend.

Backend membaca sheet `Agen`, `Grade`, `Sekolah`, dan `Plan FU` persis dengan nama tersebut. CacheService digunakan untuk master data dan LockService + batch `setValues()` digunakan saat menyimpan.

## GitHub Pages

1. Push isi folder ini ke repository GitHub.
2. Settings → Pages → Deploy from a branch → branch `main`, folder `/ (root)`.
3. Pastikan `index.html` ada di root repository yang dipilih.
4. Untuk custom path `https://rgsulselraya.xyz/followup-planner`, gunakan repository/project-site atau routing yang sudah disiapkan di domain tersebut; jangan mengubah repository/domain lain. Jika memakai project site, asset sudah relatif sehingga tetap bekerja di subpath.

## Pengujian minimum

- Buka `?route=health` dan `?route=masters` pada URL Apps Script.
- Login sebagai Agen menggunakan nama dan PIN pada sheet `Agen` dengan status Active/Aktif dan role yang diizinkan.
- Buat satu item dan beberapa item Plan FU, lalu verifikasi baris pada sheet `Plan FU`.
- Pastikan History hanya menampilkan ID agen aktif; login Admin menampilkan seluruh data dan filter.
- Periksa DevTools Network: respons masters tidak berisi PIN dan tidak ada password/credential pada file GitHub.

## Manual yang masih diperlukan

- Deploy Apps Script dan memberi izin Google saat pertama kali.
- Mengatur `ADMIN_PASSWORD` melalui `setupAdminPassword()`.
- Mengisi URL deployment `/exec` pada `API_URL`.
- Push ke repository dan mengaktifkan GitHub Pages/custom domain.
