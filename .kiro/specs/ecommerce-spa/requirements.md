# Dokumen Persyaratan

## Pendahuluan

Proyek ini adalah sebuah Prototype Website E-Commerce Single Page Application (SPA) berbasis frontend murni menggunakan HTML5, Tailwind CSS (CDN), dan JavaScript (ES6+). Aplikasi ini mensimulasikan alur belanja online lengkap mulai dari registrasi/login pengguna, penelusuran produk, pengelolaan keranjang belanja, proses checkout, hingga riwayat pesanan. Semua data disimpan di LocalStorage browser tanpa backend. Output akhir hanya terdiri dari dua file: `index.html` dan `script.js`.

## Glosarium

- **SPA**: Single Page Application — aplikasi web yang berjalan dalam satu halaman HTML tanpa reload penuh.
- **App**: Keseluruhan aplikasi e-commerce SPA ini.
- **Auth_Module**: Modul yang menangani registrasi, login, dan sesi pengguna.
- **Product_Module**: Modul yang menangani tampilan, pencarian, dan filter produk.
- **Cart_Module**: Modul yang menangani keranjang belanja.
- **Checkout_Module**: Modul yang menangani proses checkout dan pembuatan pesanan.
- **Order_Module**: Modul yang menangani tampilan riwayat pesanan.
- **Admin_Module**: Modul yang menangani panel admin untuk manajemen produk.
- **Storage**: LocalStorage browser yang digunakan sebagai penyimpanan data persisten.
- **Toast**: Notifikasi pop-up sementara yang muncul di layar.
- **Navbar**: Bilah navigasi di bagian atas halaman.
- **Modal**: Jendela dialog yang muncul di atas konten utama.
- **Dark_Mode**: Tema tampilan gelap yang dapat diaktifkan/dinonaktifkan.
- **Transaction_ID**: Identifikasi unik yang di-generate untuk setiap transaksi checkout.

---

## Persyaratan

### Persyaratan 1: Autentikasi Pengguna (Registrasi & Login)

**User Story:** Sebagai pengunjung, saya ingin mendaftar dan masuk ke akun saya, agar saya dapat berbelanja dan melihat riwayat pesanan saya.

#### Kriteria Penerimaan

1. THE App SHALL menyediakan halaman/tampilan Register dan Login yang dapat diakses dari Navbar.
2. WHEN pengguna mengisi form registrasi dengan email yang belum terdaftar dan password minimal 6 karakter, THE Auth_Module SHALL menyimpan kredensial ke Storage dengan key `users` dan menampilkan Toast sukses.
3. IF pengguna mencoba mendaftar dengan email yang sudah terdaftar di Storage, THEN THE Auth_Module SHALL menolak pendaftaran dan menampilkan pesan error "Email sudah terdaftar".
4. IF pengguna mencoba mendaftar dengan password kurang dari 6 karakter, THEN THE Auth_Module SHALL menolak pendaftaran dan menampilkan pesan error "Password minimal 6 karakter".
5. WHEN pengguna mengisi form login dengan email dan password yang cocok dengan data di Storage, THE Auth_Module SHALL membuat sesi aktif, menyimpan data sesi ke Storage dengan key `session`, dan menampilkan Toast sukses.
6. IF pengguna mencoba login dengan email atau password yang tidak cocok, THEN THE Auth_Module SHALL menolak login dan menampilkan pesan error "Email atau password salah".
7. WHEN pengguna menekan tombol Logout, THE Auth_Module SHALL menghapus data sesi dari Storage dan mengembalikan tampilan ke halaman utama.
8. WHILE sesi pengguna aktif, THE Navbar SHALL menampilkan nama pengguna atau ikon profil sebagai pengganti tombol Login.

---

### Persyaratan 2: Manajemen Produk & Tampilan

**User Story:** Sebagai pengguna, saya ingin melihat, mencari, dan memfilter produk, agar saya dapat menemukan produk yang saya inginkan dengan mudah.

#### Kriteria Penerimaan

1. THE Product_Module SHALL memuat minimal 8 produk dummy dari variabel JSON internal dengan minimal 3 kategori berbeda, masing-masing produk memiliki atribut: id, nama, harga, kategori, deskripsi, dan URL gambar.
2. THE App SHALL menampilkan produk dalam bentuk grid yang responsif di halaman utama.
3. WHEN pengguna mengetik kata kunci di Search Bar, THE Product_Module SHALL memfilter dan menampilkan hanya produk yang namanya mengandung kata kunci tersebut (case-insensitive) secara real-time.
4. WHEN pengguna memilih kategori dari filter, THE Product_Module SHALL menampilkan hanya produk yang sesuai dengan kategori tersebut.
5. WHEN pengguna mengatur rentang harga pada filter, THE Product_Module SHALL menampilkan hanya produk yang harganya berada dalam rentang tersebut.
6. WHEN filter pencarian dan kategori/harga diterapkan secara bersamaan, THE Product_Module SHALL menampilkan produk yang memenuhi semua kriteria filter secara bersamaan.
7. IF tidak ada produk yang cocok dengan filter yang diterapkan, THEN THE Product_Module SHALL menampilkan pesan "Produk tidak ditemukan" (empty state).
8. WHEN pengguna menekan tombol "Lihat Detail" pada sebuah produk, THE Product_Module SHALL menampilkan Modal atau halaman detail yang berisi informasi lengkap produk tersebut.

---

### Persyaratan 3: Sistem Keranjang Belanja

**User Story:** Sebagai pengguna yang sudah login, saya ingin mengelola keranjang belanja saya, agar saya dapat mengatur produk yang ingin saya beli sebelum checkout.

#### Kriteria Penerimaan

1. WHEN pengguna menekan tombol "Tambah ke Keranjang" pada sebuah produk, THE Cart_Module SHALL menambahkan produk tersebut ke keranjang di Storage dengan key `cart` dan menampilkan Toast konfirmasi.
2. WHEN produk yang sama ditambahkan ke keranjang lebih dari satu kali, THE Cart_Module SHALL menambah quantity produk tersebut, bukan membuat entri duplikat.
3. WHEN pengguna mengubah quantity sebuah item di halaman keranjang, THE Cart_Module SHALL memperbarui quantity dan menghitung ulang subtotal item tersebut secara real-time.
4. WHEN pengguna menekan tombol hapus pada sebuah item di keranjang, THE Cart_Module SHALL menghapus item tersebut dari keranjang dan memperbarui tampilan.
5. THE Cart_Module SHALL menghitung dan menampilkan total harga keseluruhan semua item di keranjang secara real-time.
6. THE Navbar SHALL menampilkan badge dengan jumlah total item (quantity) di keranjang yang diperbarui secara real-time.
7. IF keranjang belanja kosong, THEN THE Cart_Module SHALL menampilkan pesan "Keranjang belanja Anda kosong" (empty state).
8. THE Cart_Module SHALL mempertahankan data keranjang di Storage sehingga tidak hilang saat halaman di-refresh.

---

### Persyaratan 4: Checkout & Riwayat Pesanan

**User Story:** Sebagai pengguna, saya ingin melakukan checkout dan melihat riwayat pesanan saya, agar saya dapat menyelesaikan pembelian dan melacak transaksi saya.

#### Kriteria Penerimaan

1. WHILE keranjang belanja tidak kosong dan pengguna sudah login, THE Checkout_Module SHALL menampilkan form checkout dengan field: Nama Penerima, Alamat Pengiriman, dan Nomor HP.
2. IF pengguna mencoba checkout dengan salah satu field form kosong, THEN THE Checkout_Module SHALL menolak proses dan menampilkan pesan validasi pada field yang kosong.
3. WHEN pengguna mengisi form checkout dengan lengkap dan menekan tombol "Konfirmasi Pesanan", THE Checkout_Module SHALL men-generate Transaction_ID unik, menyimpan data pesanan ke Storage dengan key `order_history`, mengosongkan keranjang, dan menampilkan Toast sukses.
4. THE Checkout_Module SHALL memastikan setiap Transaction_ID yang di-generate bersifat unik untuk setiap transaksi.
5. THE Order_Module SHALL menampilkan semua riwayat pesanan dari Storage pada halaman "Pesanan Saya", termasuk Transaction_ID, tanggal, daftar produk, dan total harga.
6. IF pengguna belum memiliki riwayat pesanan, THEN THE Order_Module SHALL menampilkan pesan "Belum ada pesanan" (empty state).
7. IF pengguna belum login dan mencoba mengakses halaman checkout atau pesanan, THEN THE App SHALL mengarahkan pengguna ke halaman Login.

---

### Persyaratan 5: Navbar & Navigasi SPA

**User Story:** Sebagai pengguna, saya ingin navigasi yang mudah dan konsisten, agar saya dapat berpindah antar halaman dengan cepat tanpa reload.

#### Kriteria Penerimaan

1. THE Navbar SHALL selalu terlihat di bagian atas halaman dan menampilkan: Logo/nama toko, Search Bar, ikon Cart dengan badge jumlah item, dan tombol Login/profil pengguna.
2. THE App SHALL mengimplementasikan navigasi SPA dengan menampilkan dan menyembunyikan section yang relevan tanpa reload halaman penuh.
3. WHEN pengguna menekan tautan navigasi di Navbar, THE App SHALL menampilkan section yang sesuai dan menyembunyikan section lainnya.
4. THE App SHALL bersifat responsif dan dapat digunakan dengan baik di perangkat mobile maupun desktop.

---

### Persyaratan 6: Fitur Bonus — Dark Mode & Toast Notification

**User Story:** Sebagai pengguna, saya ingin pengalaman visual yang nyaman, agar saya dapat berbelanja dengan lebih menyenangkan.

#### Kriteria Penerimaan

1. THE App SHALL menyediakan tombol toggle Dark Mode di Navbar.
2. WHEN pengguna mengaktifkan Dark Mode, THE App SHALL mengubah tema tampilan ke mode gelap di seluruh halaman.
3. WHEN pengguna menonaktifkan Dark Mode, THE App SHALL mengembalikan tampilan ke mode terang.
4. THE App SHALL menyimpan preferensi Dark Mode ke Storage sehingga tetap aktif saat halaman di-refresh.
5. WHEN sebuah aksi penting berhasil dilakukan (login, tambah ke keranjang, checkout berhasil), THE App SHALL menampilkan Toast Notification yang muncul sementara lalu menghilang otomatis setelah beberapa detik.

---

### Persyaratan 7: Panel Admin

**User Story:** Sebagai admin, saya ingin mengelola daftar produk, agar saya dapat menambah atau menghapus produk dari katalog toko.

#### Kriteria Penerimaan

1. THE Admin_Module SHALL menyediakan halaman/tampilan khusus Admin Panel yang hanya dapat diakses oleh pengguna dengan role admin.
2. THE Admin_Module SHALL menampilkan form untuk menambah produk baru dengan field: nama, harga, kategori, deskripsi, dan URL gambar.
3. WHEN admin mengisi form tambah produk dengan lengkap dan menekan tombol simpan, THE Admin_Module SHALL menambahkan produk baru ke daftar produk di Storage dan memperbarui tampilan grid produk.
4. IF admin mencoba menyimpan produk dengan field wajib yang kosong, THEN THE Admin_Module SHALL menolak penyimpanan dan menampilkan pesan validasi.
5. WHEN admin menekan tombol hapus pada sebuah produk, THE Admin_Module SHALL menghapus produk tersebut dari daftar produk di Storage dan memperbarui tampilan grid produk.
6. THE App SHALL menyediakan akun admin default (misalnya email: `admin@toko.com`) yang tersimpan di data awal aplikasi.
