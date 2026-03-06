# Catatan Belajar — Panduan Teknis

Website jurnal belajar anonim dibangun dengan [Quarto](https://quarto.org).

---

## Struktur Proyek

```
zakizulham.github.io/
├── _quarto.yml              ← Konfigurasi global, tema, navbar
├── index.qmd                ← Beranda (listing + tag filter)
├── archive.qmd              ← Arsip tabel semua artikel
├── about.qmd                ← Halaman "Tentang"
├── robots.txt               ← Blokir bot AI scraper
├── internal_guide/          ← Folder panduan internal (diabaikan Quarto)
│   └── PANDUAN.md           ← Panduan teknis (file ini)
├── styles/
│   ├── custom-light.scss    ← Tema terang (Bootswatch Flatly + override)
│   ├── custom-dark.scss     ← Tema gelap (Bootswatch Darkly + override)
│   └── custom.css           ← Runtime CSS override (font, card reset)
└── posts/
    ├── _metadata.yml        ← Default frontmatter semua artikel
    ├── 2026-03-01-git-dasar/
    │   └── index.qmd        ← Contoh artikel biasa
    └── 2026-03-06-catatan-privat/
        └── index.qmd        ← Contoh artikel dengan tag noindex
```

---

## Cara Menjalankan Secara Lokal

```bash
# Pastikan Quarto sudah terinstall: https://quarto.org/docs/get-started/
quarto preview
```

Buka browser di `http://localhost:XXXX` (port ditampilkan di terminal).

---

## Cara Menambah Artikel Baru

1. Buat folder baru di `posts/`:
   ```
   posts/YYYY-MM-DD-judul-artikel/
   └── index.qmd
   ```

2. Salin template frontmatter berikut ke `index.qmd`:
   ```yaml
   ---
   title: "Judul Artikel"
   description: >
     Ringkasan singkat (1-2 kalimat) — ini yang muncul sebagai
     cuplikan di halaman beranda.
   date: YYYY-MM-DD
   categories:
     - Nama Topik
     - Tag Lain
   draft: false
   ---
   ```

3. Tulis isi artikel dalam Markdown di bawah frontmatter.

4. Jalankan `quarto preview` untuk melihat hasilnya.

> **Tips:** Nilai `description` di frontmatter adalah teks yang tampil
> sebagai *snippet* di kartu artikel pada halaman beranda. Buat ringkas
> dan deskriptif (1–2 kalimat).

---

## Privasi: Per-Halaman `noindex`

### Kapan Digunakan?

Gunakan ini untuk artikel yang ingin tetap bisa diakses via link langsung
tapi **tidak muncul di Google** dan tidak dijadikan bahan training LLM.

### Cara Menggunakannya

Tambahkan blok berikut ke frontmatter artikel yang ingin diprivasikan:

```yaml
include-in-header:
  - text: |
      <meta name="robots" content="noindex, nofollow">
```

Contoh frontmatter lengkap:

```yaml
---
title: "Refleksi Pribadi Q1 2026"
description: "Catatan internal, tidak untuk publik."
date: 2026-03-06
categories:
  - Refleksi
draft: false

include-in-header:
  - text: |
      <meta name="robots" content="noindex, nofollow">
---
```

### Apa Efeknya?

| Bot | Respons terhadap `noindex` |
|-----|--------------------------|
| Googlebot | Tidak mengindeks halaman di hasil pencarian |
| GPTBot (OpenAI) | Tidak memasukkan konten ke training data |
| Google-Extended | Tidak dipakai untuk melatih Gemini |
| CCBot | Tidak dimasukkan ke Common Crawl |
| Pengguna dengan link | **Tetap bisa mengakses halaman seperti biasa** |

> **Catatan Penting:** Tag `noindex` hanya berlaku untuk bot yang patuh
> pada standar Robots Exclusion Protocol. Bot ilegal/jahat akan
> mengabaikannya. Untuk perlindungan maksimal, gunakan kombinasi:
> 1. `robots.txt` — blokir bot di level site
> 2. `noindex` per halaman — blokir di level dokumen
> 3. Jangan publish konten sensitif sama sekali jika ingin benar-benar privat.

### Cara Memverifikasi

Buka DevTools browser → tab **Elements** → cari di dalam `<head>`:
```html
<meta name="robots" content="noindex, nofollow">
```
Jika ada, konfigurasi berjalan dengan benar.

---

## robots.txt — Pemeliharaan

File `robots.txt` memblokir bot AI training berikut:
`GPTBot`, `ChatGPT-User`, `OAI-SearchBot`, `Google-Extended`,
`anthropic-ai`, `Claude-Web`, `ClaudeBot`, `CCBot`, `cohere-ai`,
`FacebookBot`, `Applebot-Extended`, `Bytespider`, `PerplexityBot`.

**Bot scraper AI baru bermunculan secara berkala.** Pantau daftar terbaru di:
- [Dark Visitors](https://darkvisitors.com) — direktori bot AI + generator robots.txt
- [ai.robots.txt](https://github.com/ai-robots-txt/ai.robots.txt) — project komunitas open-source

---

## Deploy ke GitHub Pages

```bash
# Build site statis ke folder _site/
quarto render

# Commit dan push — GitHub Pages otomatis serve dari branch main
git add .
git commit -m "feat: tambah artikel baru"
git push
```

Pastikan GitHub Pages dikonfigurasi untuk serve dari branch `main` dan folder `/` (root) atau `_site/`.

Untuk deploy otomatis, tambahkan file `.github/workflows/publish.yml`
mengikuti [panduan resmi Quarto GitHub Pages](https://quarto.org/docs/publishing/github-pages.html).
