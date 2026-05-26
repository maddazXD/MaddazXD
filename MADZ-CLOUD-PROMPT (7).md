# MADZ-CLOUD — PROMPT PEMBANGUNAN APLIKASI LENGKAP

Kamu adalah senior full-stack developer. Tugasmu adalah membangun aplikasi **Madz-Cloud** dari nol, lengkap, production-ready, tidak ada placeholder, tidak ada `// TODO`, setiap baris kode harus fungsional.

---

## 1. DESKRIPSI PROYEK

Madz-Cloud adalah aplikasi **self-hosted file hosting** mirip catbox.moe dengan UI modern premium. User bisa upload file, mendapatkan **direct link** yang bisa langsung dipakai sebagai `<img src="...">` di HTML/CSS/project lain. Storage menggunakan **Internet Archive** (gratis unlimited). Hosting di **Vercel** (gratis). Database menggunakan **Neon** (PostgreSQL gratis).

---

## 2. TECH STACK (WAJIB PERSIS INI)

| Layer | Teknologi | Versi |
|---|---|---|
| Framework | Next.js App Router | 14.x |
| Bahasa | TypeScript | 5.x |
| Styling | Tailwind CSS | 3.x |
| UI Components | shadcn/ui | latest |
| Database ORM | Prisma | 5.x |
| Database Host | Neon (neon.tech) | PostgreSQL 16 |
| Storage | Internet Archive S3 API | - |
| Auth | Custom session + bcryptjs | - |
| AWS SDK | @aws-sdk/client-s3 | 3.x |
| Password | bcryptjs | - |
| Hosting | Vercel | - |

**Package.json dependencies wajib:**
```json
{
  "dependencies": {
    "next": "14.2.5",
    "react": "^18",
    "react-dom": "^18",
    "@prisma/client": "^5",
    "@aws-sdk/client-s3": "^3",
    "bcryptjs": "^2.4.3",
    "lucide-react": "^0.383.0",
    "tailwindcss": "^3",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "tailwind-merge": "^2.3.0"
  },
  "devDependencies": {
    "@types/bcryptjs": "^2.4.6",
    "@types/node": "^20",
    "@types/react": "^18",
    "prisma": "^5",
    "typescript": "^5"
  }
}
```

---

## 3. STRUKTUR FOLDER LENGKAP

Buat persis struktur ini, tidak kurang tidak lebih:

```
madz-cloud/
├── prisma/
│   └── schema.prisma
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   └── register/
│   │   │       └── page.tsx
│   │   ├── (dashboard)/
│   │   │   ├── layout.tsx
│   │   │   ├── dashboard/
│   │   │   │   └── page.tsx
│   │   │   ├── files/
│   │   │   │   └── page.tsx
│   │   │   ├── albums/
│   │   │   │   └── page.tsx
│   │   │   └── settings/
│   │   │       └── page.tsx
│   │   ├── f/
│   │   │   └── [shortId]/
│   │   │       └── page.tsx
│   │   ├── raw/
│   │   │   └── [shortId]/
│   │   │       └── [filename]/
│   │   │           └── route.ts
│   │   ├── api/
│   │   │   ├── auth/
│   │   │   │   └── route.ts
│   │   │   ├── files/
│   │   │   │   ├── route.ts
│   │   │   │   ├── upload/
│   │   │   │   │   ├── init/
│   │   │   │   │   │   └── route.ts
│   │   │   │   │   └── confirm/
│   │   │   │   │       └── route.ts
│   │   │   └── cron/
│   │   │       └── cleanup/
│   │   │           └── route.ts
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── ui/
│   │   │   ├── button.tsx
│   │   │   ├── input.tsx
│   │   │   ├── card.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── dialog.tsx
│   │   │   └── progress.tsx
│   │   ├── layout/
│   │   │   ├── Sidebar.tsx
│   │   │   └── Topbar.tsx
│   │   ├── files/
│   │   │   ├── FileCard.tsx
│   │   │   ├── FileRow.tsx
│   │   │   └── FileGrid.tsx
│   │   ├── modals/
│   │   │   ├── UploadModal.tsx
│   │   │   ├── RenameModal.tsx
│   │   │   ├── DeleteModal.tsx
│   │   │   ├── ShareModal.tsx
│   │   │   └── ExpiryModal.tsx
│   │   └── landing/
│   │       ├── Hero.tsx
│   │       └── DropZone.tsx
│   ├── lib/
│   │   ├── db/
│   │   │   └── prisma.ts
│   │   ├── storage/
│   │   │   └── internetArchive.ts
│   │   ├── auth/
│   │   │   └── session.ts
│   │   └── utils/
│   │       ├── format.ts
│   │       └── fileHelpers.ts
│   ├── hooks/
│   │   ├── useFiles.ts
│   │   └── useUpload.ts
│   └── types/
│       └── index.ts
├── .env.local (JANGAN di-commit)
├── .env.example
├── .gitignore
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── vercel.json
```

---

## 4. ENVIRONMENT VARIABLES

File `.env.example` berisi ini (user harus copy ke `.env.local` dan isi):

```env
# Database - Neon PostgreSQL
# Dapatkan dari: neon.tech → Create Project → Connection String
DATABASE_URL="postgresql://user:pass@ep-xxx.us-east-1.aws.neon.tech/neondb?sslmode=require"

# App URL
NEXT_PUBLIC_BASE_URL="https://madz-cloud.vercel.app"

# Session secret - generate dengan: openssl rand -hex 32
SESSION_SECRET="isi_random_string_64_karakter_disini"

# Internet Archive S3 Credentials
# Dapatkan dari: https://archive.org/account/s3.php (login dulu)
IA_ACCESS_KEY="isi_access_key_dari_internet_archive"
IA_SECRET_KEY="isi_secret_key_dari_internet_archive"
IA_ITEM_PREFIX="madz-cloud"

# Cron secret - generate dengan: openssl rand -hex 16  
CRON_SECRET="isi_random_string_untuk_cron"
```

---

## 5. DATABASE SCHEMA (prisma/schema.prisma)

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DATABASE_URL")
}

model User {
  id           String   @id @default(cuid())
  name         String
  email        String   @unique
  password     String
  storageUsed  BigInt   @default(0)
  storageLimit BigInt   @default(107374182400)
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt

  files    File[]
  albums   Album[]
  sessions Session[]
}

model Session {
  id        String   @id @default(cuid())
  userId    String
  token     String   @unique @default(cuid())
  expiresAt DateTime
  createdAt DateTime @default(now())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}

model File {
  id            String    @id @default(cuid())
  shortId       String    @unique
  userId        String?
  originalName  String
  mimeType      String
  size          BigInt
  iaIdentifier  String
  iaUrl         String
  isPublic      Boolean   @default(true)
  password      String?
  downloadCount Int       @default(0)
  expiresAt     DateTime?
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  user       User?       @relation(fields: [userId], references: [id], onDelete: SetNull)
  albumFiles AlbumFile[]

  @@index([shortId])
  @@index([userId])
  @@index([expiresAt])
}

model Album {
  id          String   @id @default(cuid())
  shortId     String   @unique
  userId      String
  title       String
  description String?
  isPublic    Boolean  @default(false)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  user       User        @relation(fields: [userId], references: [id], onDelete: Cascade)
  albumFiles AlbumFile[]

  @@index([shortId])
}

model AlbumFile {
  albumId String
  fileId  String
  addedAt DateTime @default(now())

  album Album @relation(fields: [albumId], references: [id], onDelete: Cascade)
  file  File  @relation(fields: [fileId], references: [id], onDelete: Cascade)

  @@id([albumId, fileId])
}
```

---

## 6. DESIGN SYSTEM — FINANCIAL BLUE (WAJIB PERSIS INI)

### Warna (gunakan sebagai CSS variables di globals.css):
```css
:root {
  --bg-base:      #020c1b;
  --bg-surface:   #061224;
  --bg-card:      #091a33;
  --bg-elevated:  #0d2244;
  --bg-hover:     #112a50;
  --border:       rgba(37, 99, 235, 0.16);
  --border-hover: rgba(59, 130, 246, 0.40);
  --accent:       #2563eb;
  --accent-hover: #1d4ed8;
  --accent-light: #3b82f6;
  --accent-bright:#60a5fa;
  --text-primary: #e8f0fe;
  --text-sec:     #7aa2cc;
  --text-muted:   #3b5a8a;
  --success:      #10b981;
  --danger:       #ef4444;
  --warning:      #f59e0b;
}
```

### Font: Inter (Google Fonts)
### Gradient utama: `linear-gradient(135deg, #1d4ed8, #2563eb, #3b82f6)`
### Border radius: card=16px, button=10px, input=10px, badge=20px
### Spacing: kelipatan 8px (8, 16, 24, 32, 48)

---

## 7. ALUR UPLOAD FILE (PENTING — BACA BAIK-BAIK)

Karena Vercel punya batas body size dan execution time, upload file dilakukan **langsung dari browser ke Internet Archive**, bukan melalui server Vercel. Alurnya:

```
STEP 1: User pilih file di browser
STEP 2: Browser POST /api/files/upload/init
        → Kirim: { filename, size, mimeType, isPublic, expiresAt, password }
        → Terima: { shortId, iaIdentifier, iaEndpoint, accessKey, secretKey }
STEP 3: Browser upload langsung ke Internet Archive S3 API
        → PUT https://s3.us.archive.org/{iaIdentifier}/{filename}
        → Header: Authorization (AWS Signature V4)
        → Body: file binary
STEP 4: Browser POST /api/files/upload/confirm
        → Kirim: { shortId, success: true }
        → Terima: { directLink, viewerLink, file }
```

**Catatan penting:** Di `useUpload.ts` hook, gunakan `@aws-sdk/client-s3` di browser dengan endpoint Internet Archive. Set `forcePathStyle: true`.

---

## 8. SEMUA HALAMAN DAN FITUR

### 8.1 Landing Page (`/`) — PUBLIK
- Navbar: logo "MadzCloud" (biru), tombol "Masuk" dan "Daftar Gratis"
- Hero: judul besar, subjudul, dropzone upload
- DropZone: border dashed, animasi glow saat drag, progress bar saat upload
- Upload tanpa login: dapat direct link setelah upload berhasil
- Features section: 6 card fitur (Upload Cepat, Direct Link, Password Protect, Auto Expiry, 200GB, Embeddable)
- Footer sederhana

### 8.2 Login (`/login`) dan Register (`/register`)
- Form card centered, background gelap
- Login: email + password
- Register: nama + email + password
- Tombol switch antara login/register
- Redirect ke /dashboard setelah berhasil

### 8.3 Dashboard (`/dashboard`) — PRIVATE
- 4 stat card: Total File, Storage Dipakai, Total Download, File Public
- Quick upload zone (klik untuk buka upload modal)
- Recent files: 4 file terbaru dalam grid
- Tombol "Lihat semua" → ke /files

### 8.4 My Files (`/files`) — PRIVATE
- Toolbar: filter tabs (Semua/Gambar/Video/Audio/PDF/Archive), sort (Terbaru/Terbesar), toggle grid/list
- Grid view: kartu 4 kolom, thumbnail berwarna berdasarkan tipe file
- List view: tabel dengan kolom icon, nama, tipe, ukuran, tanggal, status, aksi
- Setiap file punya menu (⋯): Copy Direct Link, Share, Rename, Atur Expiry, Jadikan Public/Private, Hapus

### 8.5 Albums (`/albums`) — PRIVATE
- Grid album, buat album baru, tambah file ke album

### 8.6 Settings (`/settings`) — PRIVATE
- Edit profil (nama, email)
- Storage usage bar dengan breakdown per tipe file
- Ganti password
- Danger zone: hapus semua file, hapus akun

### 8.7 File Viewer (`/f/[shortId]`) — PUBLIK
- Preview file (gambar tampil langsung, video ada player, audio ada player, PDF embed)
- Info: nama file, ukuran, tanggal upload, jumlah download, status expiry
- Tombol: Download, Copy Direct Link, Copy Viewer Link
- Jika ada password: form input password dulu
- Footer: "Powered by Madz-Cloud"

---

## 9. SEMUA MODAL

### 9.1 Upload Modal
- Step 1: Dropzone pilih file (drag & drop atau klik browse)
- Step 2: Opsi upload:
  - **Visibilitas**: Public / Private (toggle button)
  - **Expiry**: 2 pilihan dengan tombol radio:
    - "Tidak Pernah Hapus" (default)
    - "Auto Hapus": muncul 2 input angka: [___] hari + [___] bulan
    - Jika auto hapus dipilih, tampilkan preview: "File akan dihapus pada: Senin, 31 Januari 2026"
    - Ada tombol preset: 1 Minggu, 1 Bulan, 3 Bulan, 6 Bulan, 1 Tahun
  - **Password** (opsional): input password dengan toggle show/hide
- Progress bar real-time saat upload (persentase + kecepatan + ETA)
- Setelah berhasil: tampilkan direct link dengan tombol copy

### 9.2 Rename Modal
- Input pre-filled nama file (tanpa ekstensi)
- Ekstensi ditampilkan di samping input (tidak bisa diubah)
- Warning: "Rename akan mengubah direct link URL"
- Tombol: Batal, Simpan

### 9.3 Delete Modal
- Icon trash merah besar
- Konfirmasi nama file
- Warning: "Tindakan ini tidak dapat dibatalkan"
- Tombol: Batal, Hapus Permanen (merah)

### 9.4 Share Modal
- **Section Direct Link** (highlighted, badge "Raw File"):
  - URL: `https://madz-cloud.vercel.app/raw/[shortId]/[filename]`
  - Tombol copy
  - Label: "Gunakan untuk embed di HTML, CSS, atau project lain"
  - Contoh embed tergantung tipe file:
    - Gambar: `<img src="URL" alt="filename" />`
    - Video: `<video src="URL" controls></video>`
    - Audio: `<audio src="URL" controls></audio>`
    - Lainnya: `<a href="URL" download>filename</a>`
  - Jika gambar, tampilkan juga: `background-image: url('URL')`
  - Tombol copy untuk setiap contoh kode
- **Section Viewer Link** (badge "Preview Page"):
  - URL: `https://madz-cloud.vercel.app/f/[shortId]`
  - Tombol copy
- Info bar: status public/private, jumlah download, ukuran file

### 9.5 Expiry Modal
- Tampilkan info file (nama + expiry saat ini jika ada)
- 2 pilihan mode (tombol besar):
  - "Tidak Pernah Hapus" dengan icon bintang
  - "Auto Hapus" dengan icon jam
- Jika "Auto Hapus" dipilih:
  - Input hari (0-365) + input bulan (0-24) dalam grid 2 kolom
  - Tombol preset: 1 Minggu, 1 Bulan, 3 Bulan, 6 Bulan, 1 Tahun
  - Preview tanggal: "Akan dihapus: Senin, 31 Januari 2026"
  - Validasi: minimal 1 hari atau 1 bulan harus diisi
- Tombol: Batal, Simpan Expiry

---

## 10. SEMUA API ROUTES

### POST /api/auth
Body JSON dengan field `action`:

**action: "register"**
- Input: `{ action, name, email, password }`
- Validasi: semua wajib, password min 8 karakter, email belum terdaftar
- Hash password dengan bcrypt (cost 12)
- Buat user di DB
- Buat session, set cookie `madz_session` (httpOnly, secure, 30 hari)
- Return: `{ success: true, user: { id, name, email } }`

**action: "login"**
- Input: `{ action, email, password }`
- Cek user ada, verifikasi password
- Buat session, set cookie
- Return: `{ success: true, user: { id, name, email } }`

**action: "logout"**
- Hapus session dari DB, hapus cookie
- Return: `{ success: true }`

**action: "me"**
- Baca cookie, cari session, return user
- Return: `{ user: { id, name, email, storageUsed, storageLimit } }` atau 401

### POST /api/files/upload/init
- Auth: boleh anonim
- Input: `{ filename, size, mimeType, isPublic, expiresAt?, password? }`
- Validasi: size max 5GB, expiresAt harus masa depan
- Generate shortId (6 char hex, pastikan unik)
- Buat record File di DB dengan status pending (tambahkan field `status` di schema)
- Return: `{ shortId, iaIdentifier: "madz-cloud-{shortId}", iaEndpoint: "https://s3.us.archive.org", accessKey: process.env.IA_ACCESS_KEY, secretKey: process.env.IA_SECRET_KEY }`

### POST /api/files/upload/confirm
- Auth: boleh anonim
- Input: `{ shortId, success: boolean }`
- Jika success false: hapus record pending dari DB
- Jika success true: update record jadi aktif, hitung storage
- Return: `{ file: { shortId, originalName, directLink, viewerLink, size, mimeType, isPublic, expiresAt } }`

### GET /api/files
- Auth: wajib login
- Query params: `page`, `limit`, `search`, `filter` (all/image/video/audio/pdf/archive), `sortBy` (date/size/downloads), `sortDir` (asc/desc)
- Return: `{ files: [...], total, page, totalPages }`
- Setiap file include: id, shortId, originalName, mimeType, size (string), isPublic, hasPassword, downloadCount, expiresAt, createdAt, directLink, viewerLink

### PATCH /api/files
- Auth: wajib login, wajib pemilik file
- Body: `{ fileId, action, ...data }`
- action "rename": `{ fileId, action: "rename", newName }` → update originalName, pertahankan ekstensi
- action "setExpiry": `{ fileId, action: "setExpiry", expiresAt }` → update expiresAt (boleh null)
- action "togglePublic": `{ fileId, action: "togglePublic" }` → flip isPublic
- Return: `{ success: true, file: updatedFile }`

### DELETE /api/files
- Auth: wajib login, wajib pemilik file
- Body: `{ fileId }`
- Hapus dari DB (file di IA tidak bisa mudah dihapus — acceptable)
- Update storageUsed user
- Return: `{ success: true }`

### GET /raw/[shortId]/[filename]
- Ini adalah rute paling kritis — direct link
- Tidak perlu auth untuk file public
- Alur:
  1. Cari file di DB by shortId
  2. Return 404 jika tidak ada
  3. Cek expiresAt: jika sudah lewat, return 410 Gone
  4. Cek isPublic: jika false, cek auth, return 403 jika bukan pemilik
  5. Cek password: jika ada, baca header `X-File-Password`, verifikasi
  6. Fetch file dari Internet Archive: `GET https://archive.org/download/{iaIdentifier}/{filename}`
  7. Stream response ke user (pipe langsung, jangan buffer)
  8. Support Range header (untuk video streaming)
  9. Set headers: Content-Type, Content-Disposition (inline untuk media), Accept-Ranges, Cache-Control: public max-age=31536000, Access-Control-Allow-Origin: *
  10. Increment downloadCount (fire and forget)
- Return: file stream

### GET /api/cron/cleanup
- Header: `X-Cron-Secret` harus sama dengan env CRON_SECRET
- Cari semua file dengan expiresAt < sekarang
- Hapus dari DB (update storageUsed user)
- Return: `{ deleted: number, message: string }`

---

## 11. FILE-FILE PENTING (ISI LENGKAP)

### src/lib/db/prisma.ts
```typescript
import { PrismaClient } from '@prisma/client'
const globalForPrisma = globalThis as unknown as { prisma: PrismaClient }
export const prisma = globalForPrisma.prisma ?? new PrismaClient()
if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

### src/lib/storage/internetArchive.ts
Fungsi yang wajib ada:
- `getIADirectUrl(iaIdentifier, filename)` → string URL archive.org
- `streamFileFromIA(iaIdentifier, filename, range?)` → Response stream dari IA
  - Fetch dari `https://archive.org/download/{iaIdentifier}/{filename}`
  - Jika ada range, set header Range
  - Return response body stream
- `buildIdentifier(shortId)` → `"madz-cloud-{shortId}"`

### src/lib/auth/session.ts
Fungsi wajib:
- `hashPassword(plain)` → bcrypt hash
- `verifyPassword(plain, hash)` → boolean
- `createSession(userId)` → set cookie `madz_session`, return token
- `getAuthUser()` → AuthUser | null (baca cookie, cari session di DB)
- `destroySession()` → hapus cookie + DB
- `requireAuth()` → AuthUser atau throw 'UNAUTHORIZED'

Session cookie: httpOnly, secure (production), sameSite: lax, maxAge 30 hari

### src/lib/utils/format.ts
Fungsi wajib:
- `formatSize(bytes)` → "4.2 MB", "1.3 GB", dll
- `formatDate(date)` → "Hari ini", "Kemarin", "3 hari lalu", "12 Jan 2025"
- `formatExpiry(date)` → `{ label: "5h lagi", color: "#ef4444" }` atau null
- `addDaysAndMonths(days, months)` → Date di masa depan
- `getFileCategory(mimeType)` → "image"|"video"|"audio"|"pdf"|"text"|"archive"|"default"
- `getFileTypeColor(mimeType)` → `{ bg: string, icon: string, label: string }`

### src/hooks/useUpload.ts
Hook untuk handle upload ke Internet Archive langsung dari browser:
```typescript
// useUpload.ts harus:
// 1. Call POST /api/files/upload/init untuk dapat credentials
// 2. Upload file ke IA menggunakan fetch dengan AWS Signature V4
//    (gunakan @aws-sdk/client-s3 di browser)
//    endpoint: https://s3.us.archive.org
//    bucket: iaIdentifier
//    key: filename
// 3. Track progress upload (gunakan XMLHttpRequest untuk progress event)
// 4. Call POST /api/files/upload/confirm
// 5. Return { progress, speed, eta, status, result, error }
```

### next.config.js
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    domains: ['archive.org', 'ia800100.us.archive.org'],
  },
  async headers() {
    return [
      {
        source: '/raw/:path*',
        headers: [
          { key: 'Access-Control-Allow-Origin', value: '*' },
          { key: 'Access-Control-Allow-Methods', value: 'GET, HEAD, OPTIONS' },
        ],
      },
    ]
  },
}
module.exports = nextConfig
```

### vercel.json
```json
{
  "functions": {
    "src/app/raw/[shortId]/[filename]/route.ts": {
      "maxDuration": 60
    },
    "src/app/api/files/upload/init/route.ts": {
      "maxDuration": 30
    },
    "src/app/api/files/upload/confirm/route.ts": {
      "maxDuration": 30
    }
  }
}
```

---

## 12. KOMPONEN FILE CARD (SPESIFIKASI UI)

### FileCard (grid mode) — setiap card berisi:
- Area thumbnail atas (tinggi 120px): background berwarna sesuai tipe file
  - image: bg hijau muda, icon Image hijau
  - video: bg ungu muda, icon Video ungu
  - audio: bg kuning muda, icon Music kuning
  - pdf: bg merah muda, icon FileText merah
  - archive: bg orange muda, icon Package orange
  - default: bg biru muda, icon File biru
- Badge "Public" (hijau) di pojok kanan atas jika isPublic
- Icon Lock (kuning) di pojok kiri atas jika ada password
- Hover overlay: 3 tombol icon (Copy Direct Link, Share, Preview)
- Area info bawah:
  - Nama file (truncated dengan ellipsis), tombol ⋯ di kanan
  - Ukuran file · Tanggal upload
  - Jika ada expiry: tampilkan "Xh lagi" dengan warna (merah <3 hari, kuning <30 hari, hijau lainnya)
  - Download count dengan icon Download
  - Jika baru di-copy: tampilkan "✓ Tersalin!" sementara
- Dropdown menu dari tombol ⋯:
  - Copy Direct Link
  - Bagikan (buka Share Modal)
  - Rename
  - Atur Expiry
  - Jadikan Private / Jadikan Public
  - Hapus (merah)

### FileRow (list mode) — satu baris:
- Icon file (berwarna, background rounded)
- Nama file + info (tipe badge, public/private badge)
- Ukuran
- Tanggal
- Status expiry (jika ada)
- Download count
- Tombol aksi: Copy, Share, Edit, Clock, Hapus

---

## 13. HALAMAN FILE VIEWER (/f/[shortId])

Server component yang fetch file dari DB, lalu render:
- Jika file tidak ada: 404 page
- Jika file expired: halaman "File Kadaluarsa"
- Jika file private dan bukan pemilik: halaman "Akses Ditolak"
- Jika file ada password: tampilkan form password dulu, setelah benar baru tampilkan file
- Preview area:
  - Gambar: `<img>` full width
  - Video: `<video controls>` dengan src = direct link
  - Audio: `<audio controls>` dengan src = direct link
  - PDF: `<iframe>` embed
  - Lainnya: icon file besar dengan info
- Info file: nama, ukuran, tanggal, download count, expiry
- Tombol aksi: Download (link ke /raw/...), Copy Direct Link, Copy Viewer Link
- Footer: "Powered by Madz-Cloud"

---

## 14. SIDEBAR NAVIGASI

Lebar 240px (bisa collapse ke 64px):
- Logo "MadzCloud" (ikon Cloud + teks)
- Nav items: Dashboard, My Files, Albums, Pengaturan
- Active item: background biru transparan, border kiri biru, teks biru terang
- Hover item: background biru gelap sedikit
- Storage meter di bawah (progress bar, "X GB dari 100 GB")
- User info di paling bawah (avatar, nama, email, tombol logout)
- Di mobile: sidebar collapse jadi bottom tab bar (5 icon)

---

## 15. GITIGNORE

```
node_modules/
.next/
.env.local
.env
*.log
.DS_Store
dist/
prisma/migrations/
```

---

## 16. URUTAN IMPLEMENTASI (IKUTI PERSIS INI)

1. Setup project: `npx create-next-app@latest madz-cloud --typescript --tailwind --app --src-dir`
2. Install semua dependencies dari package.json di atas
3. Setup Prisma: `npx prisma init`, isi schema.prisma, `npx prisma generate`
4. Buat file `.env.local` dan `.env.example`
5. Buat `src/types/index.ts` (semua TypeScript interface)
6. Buat `src/lib/db/prisma.ts`
7. Buat `src/lib/auth/session.ts`
8. Buat `src/lib/utils/format.ts` dan `src/lib/utils/fileHelpers.ts`
9. Buat `src/lib/storage/internetArchive.ts`
10. Buat semua API routes (auth → files → raw)
11. Buat komponen UI (button, input, card, badge, dialog, progress)
12. Buat layout components (Sidebar, Topbar)
13. Buat modals (Upload, Rename, Delete, Share, Expiry)
14. Buat file components (FileCard, FileRow, FileGrid)
15. Buat hooks (useFiles, useUpload)
16. Buat halaman (landing → auth → dashboard → files → albums → settings → viewer)
17. Buat `next.config.js` dan `vercel.json`
18. Test semua fitur

---

## 17. CARA DEPLOY KE VERCEL

Setelah semua kode selesai dan di-push ke GitHub:

1. Pergi ke vercel.com → Import Project → pilih repo GitHub
2. Di Vercel dashboard, masuk ke Settings → Environment Variables
3. Tambahkan semua variable dari .env.example dengan nilai asli:
   - DATABASE_URL (dari neon.tech)
   - NEXT_PUBLIC_BASE_URL (URL vercel kamu, contoh: https://madz-cloud.vercel.app)
   - SESSION_SECRET
   - IA_ACCESS_KEY
   - IA_SECRET_KEY
   - IA_ITEM_PREFIX=madz-cloud
   - CRON_SECRET
4. Deploy
5. Setelah deploy, jalankan migrasi DB:
   - Di local terminal: `npx prisma migrate dev --name init`
   - Atau via Vercel CLI: `vercel env pull && npx prisma migrate deploy`

---

## 18. CARA DAPAT CREDENTIALS INTERNET ARCHIVE

1. Buka https://archive.org
2. Klik Sign Up → daftar dengan email (gratis, tidak perlu kartu kredit)
3. Setelah login, buka https://archive.org/account/s3.php
4. Klik "Generate new keys"
5. Copy Access Key dan Secret Key
6. Masukkan ke .env.local sebagai IA_ACCESS_KEY dan IA_SECRET_KEY

---

## 19. CARA DAPAT DATABASE NEON (GRATIS)

1. Buka https://neon.tech
2. Sign up dengan GitHub atau email
3. Create Project → pilih nama "madz-cloud" → region terdekat (Singapore/Tokyo)
4. Copy connection string format:
   `postgresql://user:pass@ep-xxx.region.aws.neon.tech/neondb?sslmode=require`
5. Masukkan ke .env.local sebagai DATABASE_URL

---

## 20. PENTING — ATURAN CODING

1. ZERO PLACEHOLDER: tidak ada `// TODO`, `// implement later`, atau fungsi kosong
2. Setiap fungsi harus fully implemented dan berjalan
3. Semua TypeScript types harus proper, tidak ada `any` kecuali terpaksa
4. Error handling di setiap API route (try/catch, return error JSON yang proper)
5. Loading states di semua operasi async (skeleton atau spinner)
6. Mobile responsive: sidebar collapse di mobile, grid adjust ke 2 kolom
7. Semua warna WAJIB menggunakan Design System di atas (Financial Blue)
8. Semua icon dari lucide-react
9. Tidak ada file yang tidak digunakan
10. Import paths menggunakan `@/` alias
