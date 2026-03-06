# Walkthrough — Quarto Anonymous Learning Journal

## Apa yang Dibangun

Sebuah scaffold website jurnal belajar anonim berbasis Quarto, dengan:
- Beranda listing artikel + tag filter (vanilla JS)
- Tema minimalis tipografi-first (light & dark mode)
- Sistem privasi berlapis (robots.txt + per-halaman noindex)

---

## File yang Dibuat

```
zakizulham.github.io/
├── _quarto.yml
├── index.qmd
├── archive.qmd
├── about.qmd
├── robots.txt
├── README.md
├── styles/
│   ├── custom-light.scss
│   ├── custom-dark.scss
│   └── custom.css
└── posts/
    ├── _metadata.yml
    ├── 2026-03-01-git-dasar/index.qmd
    └── 2026-03-06-catatan-privat/index.qmd
```

---

## Keputusan Desain

### Tema
- **Light:** Bootswatch `flatly` + [custom-light.scss](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/styles/custom-light.scss) override
- **Dark:** Bootswatch `darkly` + [custom-dark.scss](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/styles/custom-dark.scss) override
- Tidak ada warna aksen — semua warna berbasis abu netral
- Font: **Inter** (sans-serif) via Google Fonts + JetBrains Mono untuk kode
- Lebar kolom artikel: **max 680px** (mengikuti konvensi R4DS)
- Line-height: `1.78` untuk kenyamanan baca teks panjang

### Homepage & Search
- Quarto listing menggunakan `type: default` untuk kartu artikel dengan snippet
- Field `description` di frontmatter setiap post menjadi teks cuplikan di homepage
- Search bar bawaan Quarto aktif di navbar (site-wide)
- Tag filter custom via vanilla JS: membaca kategori dari DOM,
  menyuntikkan tombol filter, men-toggle `data-filtered` attribute pada kartu,
  CSS menyembunyikan elemen dengan `data-filtered="true"`
- Filter mendukung deep-link via URL hash: `index.html#tag=Git`

---

## Cara Kerja Privasi

### Layer 1 — [robots.txt](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/robots.txt) (site-level)
Blokir 13+ bot AI training di seluruh site. Bot yang diblokir antara lain:
`GPTBot`, `Google-Extended`, `CCBot`, `ClaudeBot`, `Bytespider`, `PerplexityBot`.

Bot patuh = tidak scrape site. Normal Googlebot tetap diizinkan.

### Layer 2 — `noindex` per-halaman (article-level)
Tambahkan ke frontmatter artikel sensitif:

```yaml
include-in-header:
  - text: |
      <meta name="robots" content="noindex, nofollow">
```

Efek: Google tidak mengindeks halaman tersebut. Artikel **tetap bisa diakses** jika link-nya diklik langsung.

**Contoh ada di:** [posts/2026-03-06-catatan-privat/index.qmd](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/posts/2026-03-06-catatan-privat/index.qmd)

---

## Cara Verifikasi Setelah `quarto preview`

| Yang Dicek | Cara Cek |
|---|---|
| Listing + snippet muncul di beranda | Buka `http://localhost:PORT` |
| Tag filter muncul dan berfungsi | Klik tombol tag → kartu artikel terfilter |
| Search bar aktif | Klik ikon kaca pembesar di navbar |
| Dark mode toggle | Klik ikon matahari/bulan di navbar |
| `noindex` pada catatan privat | DevTools → Elements → cari `<meta name="robots">` di `<head>` |
| [robots.txt](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/robots.txt) bisa diakses | `http://localhost:PORT/robots.txt` |

---

## Langkah Selanjutnya

1. Jalankan `quarto preview` dan verifikasi poin di atas
2. Hubungkan ke GitHub Pages: ikuti [panduan resmi Quarto](https://quarto.org/docs/publishing/github-pages.html)
3. Tambahkan artikel baru dengan menyalin template dari [README.md](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/README.md)
4. Pantau bot baru di [Dark Visitors](https://darkvisitors.com) dan update [robots.txt](file:///c:/Users/ZakiZ/Documents/01_Projects/zakizulham.github.io/robots.txt) secara berkala
