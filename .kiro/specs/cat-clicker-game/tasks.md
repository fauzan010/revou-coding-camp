# Implementation Plan: Cat Clicker Game

## Overview

Implementasi Cat Clicker Game sebagai aplikasi web single-page menggunakan tiga file statis: `index.html`, `style.css`, dan `script.js`. Pendekatan implementasi dimulai dari struktur file dan logika inti (pure functions yang dapat diuji), lalu dilanjutkan ke markup HTML, styling Tailwind + CSS kustom, dan akhirnya penghubungan seluruh komponen beserta test suite.

## Tasks

- [x] 1. Setup struktur file dan logika inti game (pure functions)
  - [x] 1.1 Buat file `script.js` dengan state, konstanta, dan pure functions yang dapat diuji
    - Definisikan variabel state: `let petCount = 0`, `let isAnimating = false`
    - Definisikan konstanta: `INITIAL_MESSAGE`, `REACTION_POOL`, `ANIMATION_DURATION = 200`
    - Implementasikan `createGameState(petCount, reaction)` — factory untuk state objek (digunakan untuk testing)
    - Implementasikan `getRandomReaction()` — mengembalikan string acak dari `REACTION_POOL` dengan `Math.random()`
    - Implementasikan `handleCatClick(state)` — cek `isAnimating`, increment `petCount`, set `currentReaction`
    - Implementasikan `handleReset(state)` — set `petCount = 0`, `currentReaction = INITIAL_MESSAGE`, `isAnimating = false`
    - Ekspor fungsi-fungsi tersebut dengan `export` untuk keperluan testing
    - _Requirements: 3.1, 3.4, 4.1, 4.2, 4.4, 5.4, 5.5_

  - [ ]* 1.2 Tulis property test untuk Property 1: Counter increment exactness
    - **Property 1: Counter increment exactness** — untuk sembarang `n >= 0`, klik valid menghasilkan `n + 1`
    - Buat file `tests/game.test.js` dengan setup fast-check (`npm install --save-dev fast-check vitest`)
    - `// Feature: cat-clicker-game, Property 1: Counter increment exactness`
    - **Validates: Requirements 3.1, 1.4**

  - [ ]* 1.3 Tulis property test untuk Property 2: Reaction always from pool
    - **Property 2: Reaction always from pool** — setiap klik menghasilkan reaksi dari `REACTION_POOL`
    - `// Feature: cat-clicker-game, Property 2: Reaction always from pool`
    - **Validates: Requirements 3.3**

  - [ ]* 1.4 Tulis property test untuk Property 3: Uniform reaction distribution
    - **Property 3: Uniform reaction distribution** — setelah 600 klik, setiap reaksi muncul setidaknya sekali
    - `// Feature: cat-clicker-game, Property 3: Uniform reaction distribution`
    - **Validates: Requirements 3.4**

  - [ ]* 1.5 Tulis property test untuk Property 4: Reset restores initial state
    - **Property 4: Reset restores initial state** — untuk sembarang state, reset menghasilkan state awal
    - `// Feature: cat-clicker-game, Property 4: Reset restores initial state`
    - **Validates: Requirements 4.1, 4.2**

  - [ ]* 1.6 Tulis property test untuk Property 5: Reset is idempotent
    - **Property 5: Reset is idempotent** — reset ganda pada state awal tidak mengubah apapun
    - `// Feature: cat-clicker-game, Property 5: Reset is idempotent`
    - **Validates: Requirements 4.4**

  - [ ]* 1.7 Tulis property test untuk Property 6: Animation lock prevents double-counting
    - **Property 6: Animation lock prevents double-counting** — klik ganda saat animasi hanya menghasilkan increment 1
    - `// Feature: cat-clicker-game, Property 6: Animation lock prevents double-counting`
    - **Validates: Requirements 3.5**

- [-] 2. Checkpoint — Pastikan semua property tests lulus
  - Jalankan `npx vitest --run` untuk memverifikasi seluruh property test hijau sebelum melanjutkan ke HTML/CSS
  - Pastikan semua tests lulus, tanya pengguna jika ada pertanyaan.

- [ ] 3. Buat markup HTML (`index.html`)
  - [~] 3.1 Buat file `index.html` dengan struktur halaman lengkap
    - Tambahkan `<!DOCTYPE html>`, `<html lang="id">`, `<head>` dengan `<meta charset>`, `<meta viewport>`, dan `<title>`
    - Tambahkan satu tag `<script src="https://cdn.tailwindcss.com">` di `<head>`
    - Tambahkan `<link rel="stylesheet" href="style.css">` di `<head>`
    - Tambahkan `<script src="script.js" type="module" defer>` di akhir `<body>`
    - Buat elemen judul (`<h1>`) maksimal 60 karakter dan deskripsi (`<p>`) maksimal 150 karakter
    - Buat `<div id="cat-display">` yang memuat inline SVG kucing minimalis (telinga, kepala, mata, hidung, kumis, mulut) dengan warna `#f5a623`
    - Buat `<div id="reaction-bubble">` dengan `<span id="reaction-text">Halo! Yuk elus aku!</span>` di atas `cat-display`
    - Buat `<div id="pet-counter">` dengan `<span id="counter-value">0</span>`
    - Buat `<button id="reset-btn">Reset</button>`
    - Tambahkan atribut `onclick="handleCatClick()"` pada `cat-display` dan `onclick="handleReset()"` pada `reset-btn`
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 1.6, 2.1, 2.2, 5.1, 5.2, 5.3_

  - [ ]* 3.2 Tulis unit tests untuk DOM structure
    - Verifikasi elemen `#cat-display`, `#counter-value`, `#reaction-text`, `#reset-btn` ada di DOM menggunakan jsdom
    - Verifikasi nilai awal counter = "0" dan teks reaksi = "Halo! Yuk elus aku!"
    - Verifikasi tag `<script src="https://cdn.tailwindcss.com">` ada di `<head>`
    - _Requirements: 1.3, 1.4, 1.5, 1.6, 2.1, 2.2, 5.3_

- [ ] 4. Buat styling (`style.css`)
  - [~] 4.1 Buat file `style.css` dengan layout dan visual utama
    - Implementasikan background gradasi linear dua warna pastel (misal: `#ffecd2` → `#fcb69f`)
    - Implementasikan centering layout: `min-height: 100vh`, `display: flex`, `align-items: center`, `justify-content: center`
    - Implementasikan kartu utama dengan `border-radius >= 8px`, `box-shadow` yang terlihat, lebar maksimal sesuai
    - Implementasikan font modern yang konsisten (Google Fonts atau font-stack yang sesuai)
    - Implementasikan responsive layout: pada `max-width: 767px` lebar konten `max-width: 480px`
    - Pastikan rasio kontras teks ≥ 4.5:1 (WCAG AA) menggunakan warna gelap pada latar terang
    - Definisikan class `.cat-animate` untuk Click_Animation: `transform: scale(1.2)` selama 100ms lalu kembali ke `scale(1)` dalam 100ms
    - Implementasikan styling untuk `#reaction-bubble`, `#pet-counter`, `#reset-btn` dengan `border-radius >= 8px`
    - _Requirements: 1.7, 1.8, 1.9, 1.10, 1.11, 1.12, 1.13, 1.14, 3.2_

- [ ] 5. Wiring DOM — sambungkan script.js ke HTML
  - [~] 5.1 Tambahkan fungsi `updateDisplay()` dan `playAnimation()` di `script.js`
    - Implementasikan `updateDisplay()` — baca `petCount` dan `currentReaction` dari state, perbarui `#counter-value` dan `#reaction-text` di DOM
    - Implementasikan `playAnimation()` — tambahkan class `.cat-animate` ke `#cat-display`, set `isAnimating = true`, panggil `setTimeout(200ms)` untuk hapus class dan set `isAnimating = false`
    - Ubah `handleCatClick()` (versi DOM) — panggil logika pure function lalu `updateDisplay()` dan `playAnimation()`
    - Ubah `handleReset()` (versi DOM) — panggil logika pure function lalu `updateDisplay()` dan pastikan animasi dihentikan (hapus class `.cat-animate`, reset `isAnimating = false`) dalam ≤ 100ms
    - Pastikan `handleCatClick` dan `handleReset` terdaftar sebagai event listener pada elemen DOM yang sesuai
    - _Requirements: 2.3, 3.1, 3.2, 3.3, 3.5, 4.1, 4.2, 4.3_

  - [ ]* 5.2 Tulis unit tests untuk DOM integration
    - Test bahwa klik pada `#cat-display` memperbarui `#counter-value` di DOM
    - Test bahwa klik pada `#reset-btn` mengembalikan `#counter-value` ke "0" dan `#reaction-text` ke `INITIAL_MESSAGE`
    - Test bahwa class `.cat-animate` ditambahkan saat klik dan dihapus setelah 200ms
    - Test bahwa klik ganda cepat (< 200ms) hanya menginkremen counter satu kali
    - Test bahwa `script.js` tidak menggunakan `localStorage` (grep/regex check)
    - _Requirements: 3.1, 3.2, 3.5, 4.1, 4.2, 4.3, 5.5_

- [~] 6. Final checkpoint — Pastikan semua tests lulus dan game berjalan
  - Jalankan `npx vitest --run` untuk memverifikasi seluruh test suite (property tests + unit tests) hijau
  - Buka `index.html` langsung di browser melalui protokol `file://` untuk verifikasi manual
  - Pastikan semua tests lulus, tanya pengguna jika ada pertanyaan.

## Notes

- Tasks bertanda `*` bersifat opsional dan dapat dilewati untuk implementasi MVP yang lebih cepat
- Setiap task mereferensikan requirement spesifik untuk keterlacakan
- Checkpoint memastikan validasi inkremental sebelum melanjutkan ke fase berikutnya
- Property tests (fast-check) memvalidasi properti kebenaran universal (6 properti)
- Unit tests memvalidasi contoh spesifik dan kondisi tepi
- Fungsi pure (`createGameState`, `getRandomReaction`, `handleCatClick`, `handleReset`) dipisahkan dari DOM manipulation agar dapat diuji di Node.js tanpa browser
- Setup testing: `npm init -y && npm install --save-dev vitest fast-check` lalu tambahkan `"test": "vitest --run"` di `package.json`

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1.1"] },
    { "id": 1, "tasks": ["1.2", "1.3", "1.4", "1.5", "1.6", "1.7"] },
    { "id": 2, "tasks": ["3.1"] },
    { "id": 3, "tasks": ["3.2", "4.1"] },
    { "id": 4, "tasks": ["5.1"] },
    { "id": 5, "tasks": ["5.2"] }
  ]
}
```
