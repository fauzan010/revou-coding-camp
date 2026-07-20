# Requirements Document

## Introduction

Cat Clicker Game adalah sebuah website single-page interaktif bertema kucing yang memungkinkan pengguna mengelus (pet) seekor kucing lucu bergaya minimalis. Setiap kali kucing diklik, counter pet bertambah dan kucing memberikan reaksi lucu secara acak melalui bubble teks. Aplikasi dibangun menggunakan HTML5, Tailwind CSS, dan Vanilla JavaScript tanpa backend atau database, sehingga dapat dijalankan langsung di browser.

## Glossary

- **Game**: Aplikasi web single-page cat clicker yang berjalan di browser
- **Cat_Display**: Komponen ilustrasi SVG kucing minimalis yang dapat diklik oleh pengguna
- **Pet_Counter**: Komponen yang menampilkan dan menyimpan jumlah total klik/pet selama sesi berlangsung
- **Reaction_Bubble**: Komponen bubble teks yang tampil di atas Cat_Display dan menampilkan reaksi kucing
- **Reset_Button**: Tombol yang mereset Pet_Counter ke 0 dan Reaction_Bubble ke pesan awal
- **Click_Animation**: Animasi singkat pada Cat_Display saat diklik (membesar lalu kembali normal)
- **Reaction_Pool**: Kumpulan teks reaksi yang dapat dipilih secara acak: "Meow~", "Purrr...", "Nyan~", "Aku senang!", "Lagi dong!", "😺"
- **Initial_Message**: Pesan awal yang ditampilkan Reaction_Bubble sebelum kucing diklik atau setelah reset: "Halo! Yuk elus aku!"
- **Page**: Halaman web utama tempat seluruh komponen Game ditampilkan

---

## Requirements

### Requirement 1: Tampilan Halaman Utama

**User Story:** Sebagai pengguna, saya ingin melihat halaman yang bersih dan menarik secara visual, sehingga saya dapat menikmati pengalaman bermain yang menyenangkan.

#### Acceptance Criteria

1. THE Page SHALL menampilkan judul website (tidak lebih dari 60 karakter) di bagian atas konten utama.
2. THE Page SHALL menampilkan deskripsi singkat (tidak lebih dari 150 karakter) di bawah judul website.
3. THE Page SHALL menampilkan Cat_Display berupa ilustrasi SVG kucing bergaya minimalis di tengah halaman.
4. THE Page SHALL menampilkan Pet_Counter yang menunjukkan bilangan bulat non-negatif yang merepresentasikan jumlah total pet.
5. THE Page SHALL menampilkan Reaction_Bubble di atas Cat_Display tanpa menutupi area kepala ilustrasi kucing.
6. THE Page SHALL menampilkan Reset_Button yang dapat diklik.
7. THE Page SHALL memusatkan (center) seluruh konten secara horizontal dan vertikal terhadap viewport.
8. THE Page SHALL menerapkan warna teks dan latar belakang dengan rasio kontras minimal 4.5:1 (WCAG AA) pada seluruh teks utama.
9. THE Page SHALL menerapkan background dengan gradasi linear menggunakan tepat dua warna pastel.
10. THE Page SHALL menerapkan satu jenis font modern yang konsisten pada seluruh elemen teks di halaman.
11. THE Page SHALL menerapkan border-radius minimal 8px pada semua elemen kartu dan tombol.
12. THE Page SHALL menerapkan box-shadow yang dapat terlihat secara visual pada elemen kartu utama.
13. IF lebar viewport kurang dari 768px, THE Page SHALL menampilkan layout dalam satu kolom vertikal dengan lebar maksimal 480px.
14. IF lebar viewport lebih dari atau sama dengan 768px, THE Page SHALL menampilkan konten terpusat dengan lebar maksimal yang sesuai untuk layar besar.

---

### Requirement 2: Kondisi Awal Game

**User Story:** Sebagai pengguna, saya ingin melihat kondisi awal yang jelas saat pertama kali membuka halaman, sehingga saya langsung memahami cara berinteraksi dengan game.

#### Acceptance Criteria

1. WHEN halaman pertama kali dimuat, THE Pet_Counter SHALL menampilkan nilai 0.
2. WHEN halaman pertama kali dimuat, THE Reaction_Bubble SHALL menampilkan teks "Halo! Yuk elus aku!" secara verbatim.
3. WHEN halaman pertama kali dimuat, THE Cat_Display SHALL ditampilkan dalam kondisi diam tanpa animasi aktif (tidak ada transformasi scale, tidak ada transisi yang sedang berjalan).
4. WHEN halaman di-reload (refresh), THE Game SHALL kembali ke kondisi awal dengan Pet_Counter = 0 dan Reaction_Bubble menampilkan "Halo! Yuk elus aku!".

---

### Requirement 3: Interaksi Klik pada Kucing

**User Story:** Sebagai pengguna, saya ingin mengklik kucing dan mendapatkan respons visual yang menarik, sehingga saya merasa berinteraksi dengan kucing yang hidup.

#### Acceptance Criteria

1. WHEN pengguna mengklik Cat_Display, THE Pet_Counter SHALL bertambah sebesar 1.
2. WHEN pengguna mengklik Cat_Display, THE Cat_Display SHALL memainkan Click_Animation: scale membesar dari 100% ke 120% dalam 100ms, lalu kembali ke 100% dalam 100ms berikutnya.
3. WHEN pengguna mengklik Cat_Display, THE Reaction_Bubble SHALL menampilkan satu teks yang dipilih secara acak dari Reaction_Pool dan teks tersebut SHALL tetap tampil minimal selama 1 detik.
4. WHEN pengguna mengklik Cat_Display, THE Game SHALL memilih teks dari Reaction_Pool menggunakan distribusi seragam sehingga setiap dari 6 teks memiliki peluang 1/6 untuk dipilih pada setiap klik.
5. WHILE Click_Animation sedang berjalan, THE Game SHALL mengabaikan klik tambahan pada Cat_Display hingga animasi selesai sepenuhnya (total 200ms sejak klik pertama).

---

### Requirement 4: Tombol Reset

**User Story:** Sebagai pengguna, saya ingin dapat mereset game kapan saja, sehingga saya dapat memulai kembali dari awal.

#### Acceptance Criteria

1. WHEN pengguna mengklik Reset_Button, THE Pet_Counter SHALL kembali ke nilai 0.
2. WHEN pengguna mengklik Reset_Button, THE Reaction_Bubble SHALL menggantikan teks yang sedang ditampilkan (apapun isinya) dengan teks "Halo! Yuk elus aku!" secara verbatim.
3. WHEN pengguna mengklik Reset_Button, THE Cat_Display SHALL menghentikan animasi yang sedang berjalan dalam waktu maksimal 100ms dan kembali ke kondisi diam (scale 100%, tidak ada transformasi aktif).
4. WHEN pengguna mengklik Reset_Button dan game sudah dalam kondisi awal (Pet_Counter = 0 dan Reaction_Bubble menampilkan "Halo! Yuk elus aku!"), THE Game SHALL tetap mempertahankan kondisi awal tanpa perubahan.

---

### Requirement 5: Struktur dan Kompatibilitas Kode

**User Story:** Sebagai developer, saya ingin kode terorganisasi dalam file terpisah dan berjalan langsung di browser, sehingga mudah dipelihara dan tidak memerlukan proses build atau server.

#### Acceptance Criteria

1. THE Game SHALL disusun dalam tiga file terpisah: `index.html`, `style.css`, dan `script.js`, di mana `index.html` me-link `style.css` melalui elemen `<link>` dan `script.js` melalui elemen `<script>`.
2. THE Game SHALL dapat dijalankan langsung di browser modern (Chrome, Firefox, Safari, Edge versi terbaru) melalui protokol `file://` hanya dengan membuka `index.html`, tanpa server backend, database, atau proses build.
3. THE Game SHALL menggunakan Tailwind CSS melalui satu tag `<script>` CDN di `index.html` tanpa file konfigurasi Tailwind lokal.
4. THE `script.js` SHALL tidak memuat library atau framework JavaScript eksternal tambahan selain logika game itu sendiri, baik melalui CDN maupun file lokal.
5. THE Game SHALL menyimpan state (Pet_Counter sebagai bilangan bulat dan Reaction_Bubble sebagai string) sepenuhnya dalam memori JavaScript, tanpa menggunakan localStorage, sessionStorage, IndexedDB, cookie, atau komunikasi jaringan apapun.
6. WHEN sesi browser ditutup atau halaman di-refresh, THE Game SHALL kehilangan semua state (Pet_Counter kembali ke 0) karena state hanya disimpan di memori, bukan di storage persisten.
