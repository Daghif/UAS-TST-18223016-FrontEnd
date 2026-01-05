# UAS-TST-18223016-FrontEnd

**BookSocial - Integrated Social Library Platform**

Aplikasi web Review Buku modern yang dibangun menggunakan Vanilla JavaScript dan Bootstrap 5. Frontend ini berinteraksi dengan REST API backend untuk pengelolaan buku, review komunitas, dan koleksi personal, serta mengintegrasikan **Microservice External** dari partner (Perpustakaan Ayam Jago) untuk menampilkan ketersediaan stok buku fisik secara real-time.

**Live Demo:** [https://uas-tst-18223016-front-end.vercel.app](https://uas-tst-18223016-front-end.vercel.app)

---

## 📋 Fitur Utama

### 1. Otentikasi & Keamanan
- **Login & Register:** Sistem otentikasi menggunakan **JWT (JSON Web Token)**.
- **Auto Redirect:** Proteksi halaman (user tanpa token otomatis diarahkan ke login).
- **Session Handling:** Token disimpan aman di LocalStorage dan divalidasi setiap request.

### 2. Manajemen Buku (Dashboard)
- **Katalog Buku:** Menampilkan daftar buku dengan pagination dan average rating.
- **Pencarian (Search):** Mencari buku berdasarkan judul secara real-time.
- **Detail Buku:** Melihat informasi lengkap buku beserta review komunitas.

### 3. Integrasi Microservice (External API Partner)
Fitur unggulan yang mengambil data dari sumber eksternal **tanpa membebani backend utama**:

- Setiap kartu buku memiliki link **"Cek Ketersediaan di Perpus Ayam Jago"**.
- Melakukan `asynchronous request` ke API partner: `https://arya.theokaitou.my.id/api/books?search={title}`.
- Menampilkan **status stok fisik** (Tersedia/Habis/Tidak Ada) secara real-time.
- Fallback tampilan jika koneksi partner gagal.

### 4. Review & Interaksi Sosial
- **Tulis Review:** Memberikan rating (1-5 bintang) dan komentar pada buku.
- **Lihat Review Komunitas:** Melihat ulasan dari pengguna lain di halaman detail buku.
- **My Reviews:** Halaman khusus untuk melihat riwayat review pribadi dengan statistik (total review, rata-rata rating, jumlah buku direview).

### 5. Koleksi Personal (Shelves)
- **Buat Rak Buku:** Membuat kategori rak kustom (contoh: "Favorit", "Wishlist", "Skripsi").
- **Tambah Buku ke Rak:** Menyimpan buku ke rak yang dipilih.
- **Hapus Buku dari Rak:** Mengeluarkan buku dari koleksi tanpa menghapus dari katalog.
- **Hapus Rak:** Menghapus seluruh rak beserta isinya (cascade delete).
- **Statistik Koleksi:** Menampilkan total rak dan total buku yang disimpan.

### 6. User Experience & Navigation
- **Responsive Design:** Tampilan optimal di desktop, tablet, dan mobile.
- **Loading States:** Spinner animasi saat fetching data untuk feedback visual.
- **Empty States:** Pesan informatif dengan CTA button untuk state kosong (belum ada review/rak).
- **Toast Notifications:** Notifikasi sukses/error yang muncul di pojok layar.
- **Modal Overlay:** Form input review dan pilihan rak menggunakan modal yang tidak mengganggu browsing.

---

## 🛠️ Teknologi yang Digunakan

- **Core:** HTML5, CSS3, JavaScript (ES6+).
- **Styling:** Custom CSS dengan CSS Variables untuk theming konsisten.
- **Icons:** [FontAwesome 6.0](https://fontawesome.com/) (CDN).
- **HTTP Client:** Fetch API (Native Browser).
- **Deployment:** [Vercel](https://vercel.com/) (Static Site Hosting).

---

## 🚀 Integrasi API

Frontend ini menghubungkan **dua layanan API** yang berbeda:

| Layanan | Endpoint Base URL | Fungsi |
|---------|-------------------|--------|
| **Main Backend** | `https://thegift.theokaitou.my.id/api` | Auth, CRUD Buku, Reviews, Shelves |
| **Partner API (Ayam Jago)** | `https://arya.theokaitou.my.id/api` | Cek stok fisik buku di perpustakaan kampus |

### Contoh Request Integrasi Partner:

```javascript
// Cek ketersediaan buku "On Palestine" di perpustakaan fisik
const res = await fetch('https://arya.theokaitou.my.id/api/books?search=On Palestine');
const data = await res.json();

// Response:
{
  "data": [
    {
      "id": 101,
      "title": "On Palestine",
      "author": "Noam Chomsky",
      "stock": 5,  // Stok fisik tersedia
      "description": "..."
    }
  ]
}
