# Rencana Implementasi: ecommerce-spa

## Ikhtisar

Implementasi dilakukan secara bertahap dalam dua file: `index.html` dan `script.js`. Setiap task membangun di atas task sebelumnya, dimulai dari struktur dasar hingga fitur lengkap yang terhubung satu sama lain.

## Tasks

- [ ] 1. Buat struktur dasar index.html dengan Tailwind CSS CDN
  - Buat file `index.html` dengan boilerplate HTML5
  - Tambahkan Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com">`)
  - Buat semua section SPA: `#page-home`, `#page-login`, `#page-register`, `#page-cart`, `#page-checkout`, `#page-orders`, `#page-admin`, `#modal-product`
  - Buat Navbar dengan: logo, search bar, badge cart, tombol dark mode, tombol login/profil
  - Buat `#toast-container` untuk notifikasi
  - Semua section kecuali `#page-home` diberi `class="hidden"`
  - _Persyaratan: 5.1, 5.2, 5.4_

- [ ] 2. Implementasi Storage Module dan UI Module dasar di script.js
  - [ ] 2.1 Buat file `script.js` dan implementasi Storage Module
    - Definisikan konstanta `KEYS` untuk semua kunci LocalStorage
    - Implementasi `storageGet(key)` dan `storageSet(key, value)`
    - _Persyaratan: 3.8, 4.3_
  - [ ] 2.2 Implementasi UI Module: navigate, toast, dark mode
    - Implementasi `navigate(page)` untuk show/hide section
    - Implementasi `showToast(message, type)` dengan auto-dismiss
    - Implementasi `toggleDarkMode()` dan `initDarkMode()` yang membaca/menulis Storage
    - Hubungkan tombol dark mode di Navbar ke `toggleDarkMode()`
    - _Persyaratan: 5.2, 5.3, 6.1, 6.2, 6.3, 6.4_
  - [ ]* 2.3 Tulis property test untuk persistensi Dark Mode
    - **Properti 14: Preferensi Dark Mode persisten di Storage**
    - **Memvalidasi: Persyaratan 6.4**

- [ ] 3. Implementasi Auth Module
  - [ ] 3.1 Implementasi fungsi autentikasi inti
    - Implementasi `authRegister(name, email, password)` dengan validasi email unik dan password >= 6 karakter
    - Implementasi `authLogin(email, password)` yang membuat sesi di Storage
    - Implementasi `authLogout()` yang menghapus sesi dari Storage
    - Implementasi `authGetSession()` dan `authIsAdmin()`
    - Inisialisasi data admin default (`admin@toko.com`) jika belum ada di Storage
    - _Persyaratan: 1.2, 1.3, 1.4, 1.5, 1.6, 1.7, 7.1_
  - [ ] 3.2 Buat form Register dan Login di index.html dan hubungkan ke Auth Module
    - Isi section `#page-register` dengan form (nama, email, password)
    - Isi section `#page-login` dengan form (email, password)
    - Hubungkan submit form ke `authRegister()` dan `authLogin()`
    - Tampilkan pesan error inline dan Toast sukses
    - Implementasi `updateNavbar()` untuk update tampilan setelah login/logout
    - _Persyaratan: 1.1, 1.8, 5.1_
  - [ ]* 3.3 Tulis property test untuk Auth Module
    - **Properti 1: Registrasi valid menyimpan user ke Storage**
    - **Properti 2: Email duplikat ditolak**
    - **Properti 3: Password pendek ditolak**
    - **Properti 4: Round-trip sesi (login → logout)**
    - **Properti 5: Login dengan kredensial salah ditolak**
    - **Memvalidasi: Persyaratan 1.2, 1.3, 1.4, 1.5, 1.6, 1.7**

- [ ] 4. Checkpoint — Pastikan semua test lulus, tanyakan kepada pengguna jika ada pertanyaan.

- [ ] 5. Implementasi Product Module
  - [ ] 5.1 Definisikan data produk default dan implementasi fungsi produk inti
    - Definisikan array `DEFAULT_PRODUCTS` dengan 8 produk (4 kategori: Pakaian, Elektronik, Sepatu, Tas)
    - Implementasi `productGetAll()` yang membaca dari Storage atau menggunakan default
    - Implementasi `productFilter(query, category, minPrice, maxPrice)` dengan logika AND untuk semua filter
    - _Persyaratan: 2.1, 2.3, 2.4, 2.5, 2.6_
  - [ ] 5.2 Implementasi render grid produk dan modal detail
    - Implementasi `productRenderGrid(products)` yang menghasilkan HTML card produk responsif
    - Implementasi `productShowModal(productId)` yang mengisi dan menampilkan `#modal-product`
    - Tambahkan event listener untuk search bar, filter kategori, filter harga di Navbar/halaman home
    - Tampilkan empty state jika hasil filter kosong
    - _Persyaratan: 2.2, 2.7, 2.8_
  - [ ]* 5.3 Tulis property test untuk Product Module
    - **Properti 6: Filter produk komprehensif (nama + kategori + harga)**
    - **Memvalidasi: Persyaratan 2.3, 2.4, 2.5, 2.6**

- [ ] 6. Implementasi Cart Module
  - [ ] 6.1 Implementasi fungsi cart inti
    - Implementasi `cartGet()`, `cartAdd(productId)`, `cartUpdateQty(productId, qty)`, `cartRemove(productId)`, `cartClear()`
    - Implementasi `cartGetTotal()` dengan formula `sum(item.price * item.quantity)`
    - Implementasi `cartGetItemCount()` untuk badge Navbar
    - Pastikan semua operasi menulis ke Storage
    - _Persyaratan: 3.1, 3.2, 3.3, 3.4, 3.5, 3.8_
  - [ ] 6.2 Implementasi render halaman cart dan update Navbar badge
    - Implementasi `cartRender()` yang menghasilkan HTML daftar item cart dengan kontrol quantity
    - Hubungkan tombol +/- quantity dan hapus item ke fungsi cart
    - Update badge Navbar setiap kali cart berubah
    - Tampilkan empty state jika cart kosong
    - Hubungkan tombol "Tambah ke Keranjang" di product card ke `cartAdd()`
    - _Persyaratan: 3.6, 3.7_
  - [ ]* 6.3 Tulis property test untuk Cart Module
    - **Properti 7: Round-trip cart (tambah → hapus)**
    - **Properti 8: Duplikat menambah quantity, bukan entri baru**
    - **Properti 9: Invariant total harga cart**
    - **Properti 10: Persistensi cart di Storage**
    - **Memvalidasi: Persyaratan 3.1, 3.2, 3.3, 3.4, 3.5, 3.8**

- [ ] 7. Checkpoint — Pastikan semua test lulus, tanyakan kepada pengguna jika ada pertanyaan.

- [ ] 8. Implementasi Checkout Module dan Order Module
  - [ ] 8.1 Implementasi Checkout Module
    - Implementasi `generateTransactionId()` menggunakan `Date.now()` + random suffix
    - Implementasi `checkoutProcess(name, address, phone)` dengan validasi field wajib
    - Setelah checkout berhasil: simpan order ke `order_history`, panggil `cartClear()`, tampilkan Toast sukses, navigate ke halaman orders
    - _Persyaratan: 4.2, 4.3, 4.4_
  - [ ] 8.2 Buat form Checkout di index.html dan hubungkan ke Checkout Module
    - Isi section `#page-checkout` dengan form (Nama Penerima, Alamat, No HP) dan ringkasan order
    - Tambahkan guard: redirect ke login jika belum login, redirect ke home jika cart kosong
    - _Persyaratan: 4.1, 4.7_
  - [ ] 8.3 Implementasi Order Module dan halaman "Pesanan Saya"
    - Implementasi `orderGetAll()` dan `orderRender()`
    - Isi section `#page-orders` dengan render riwayat pesanan
    - Tampilkan empty state jika belum ada pesanan
    - Tambahkan guard: redirect ke login jika belum login
    - _Persyaratan: 4.5, 4.6, 4.7_
  - [ ]* 8.4 Tulis property test untuk Checkout Module
    - **Properti 11: Checkout berhasil mengosongkan cart dan menyimpan order**
    - **Properti 12: Transaction_ID selalu unik**
    - **Memvalidasi: Persyaratan 4.3, 4.4**

- [ ] 9. Implementasi Admin Module
  - [ ] 9.1 Implementasi fungsi admin inti
    - Implementasi `productAdd(product)` yang menambah produk ke Storage
    - Implementasi `productDelete(productId)` yang menghapus produk dari Storage
    - Implementasi `adminRender()` yang menampilkan daftar produk dan form tambah produk
    - _Persyaratan: 7.2, 7.3, 7.5_
  - [ ] 9.2 Buat halaman Admin Panel di index.html dan hubungkan ke Admin Module
    - Isi section `#page-admin` dengan form tambah produk dan tabel daftar produk
    - Tambahkan guard: hanya bisa diakses jika `authIsAdmin()` true
    - Hubungkan form submit ke `adminAddProduct()` dengan validasi field wajib
    - Hubungkan tombol hapus ke `productDelete()` dan refresh tampilan
    - _Persyaratan: 7.1, 7.4, 7.6_
  - [ ]* 9.3 Tulis property test untuk Admin Module
    - **Properti 13: Round-trip manajemen produk admin (tambah → hapus)**
    - **Memvalidasi: Persyaratan 7.3, 7.5**

- [ ] 10. Integrasi akhir dan polish UI
  - [ ] 10.1 Hubungkan semua modul dan pastikan navigasi SPA berfungsi penuh
    - Pastikan semua link Navbar memanggil `navigate()` dengan page yang benar
    - Pastikan `updateNavbar()` dipanggil setelah setiap perubahan state (login, logout, cart update)
    - Panggil `initDarkMode()` dan inisialisasi produk default saat halaman pertama kali dimuat
    - _Persyaratan: 5.2, 5.3_
  - [ ] 10.2 Implementasi responsivitas dan polish Tailwind CSS
    - Pastikan grid produk responsif (1 kolom mobile, 2-3 kolom tablet, 4 kolom desktop)
    - Pastikan Navbar responsif dengan hamburger menu jika diperlukan
    - Pastikan semua empty state ditampilkan dengan desain yang bersih
    - _Persyaratan: 5.4_

- [ ] 11. Checkpoint akhir — Pastikan semua test lulus dan semua fitur terintegrasi dengan baik, tanyakan kepada pengguna jika ada pertanyaan.

## Catatan

- Task bertanda `*` bersifat opsional dan dapat dilewati untuk MVP yang lebih cepat
- Setiap task mereferensikan persyaratan spesifik untuk keterlacakan
- Checkpoint memastikan validasi bertahap
- Property test memvalidasi properti kebenaran universal
- Unit test memvalidasi contoh spesifik dan edge case
