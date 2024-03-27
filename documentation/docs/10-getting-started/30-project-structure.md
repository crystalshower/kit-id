---
title: Struktur proyek
---

Ketika kamu membuat proyek SvelteKit, kamu akan mendapatkan struktur direktori seperti ini:

```bash
my-project/
├ src/
│ ├ lib/
│ │ ├ server/
│ │ │ └ [file-file lib server kamu]
│ │ └ [file-file lib kamu]
│ ├ params/
│ │ └ [file-file param kamu]
│ ├ routes/
│ │ └ [file-file route kamu]
│ ├ app.html
│ ├ error.html
│ ├ hooks.client.js
│ ├ hooks.server.js
│ └ service-worker.js
├ static/
│ └ [file-file static kamu]
├ tests/
│ └ [file-file test kamu]
├ package.json
├ svelte.config.js
├ tsconfig.json
└ vite.config.js
```

Kamu juga akan menemukan file-file umum seperti `.gitignore` dan `.npmrc` (`.prettierrc`, `.eslintrc.cjs` dan lain-lain, jika kamu memilih opsi tersebut ketika menjalankan `npm create svelte@latest`).

## File proyek

### src

Direktori `src` berisi inti dari proyek kamu. Semuanya kecuali `src/routes` dan `src/app.html` adalah opsional.

- `lib` berisi kode pustaka kamu (utilitas dan komponen), yang dapat diimpor melalui alias [`$lib`](modules#$lib), atau dikemas untuk didistribusikan menggunakan [`svelte-package`](packaging)
  - `server` berisi kode pustaka yang hanya berjalan di server. Kode ini dapat diimpor dengan menggunakan alias [`$lib/server`](server-only-modules). SvelteKit akan mencegah kamu mengimpor kode ini di kode klien.
- `params` berisi [param matcher](advanced-routing#matching) yang dibutuhkan
- `routes` berisi [route](routing) aplikasi yang kamu buat. Kamu juga dapat menempatkan komponen lain yang hanya digunakan dalam satu route di sini
- `app.html` adalah template halaman kamu — sebuah dokumen HTML yang berisi placeholder berikut:
  - `%sveltekit.head%` — `<link>` dan `<script>` yang dibutuhkan aplikasimu, termasuk kode yang dihasilkan oleh `<svelte:head>`
  - `%sveltekit.body%` — markup untuk halaman yang di-render. Bagian ini harus berada di dalam `<div>` atau elemen lain, bukan langsung di dalam `<body>`, untuk mencegah bug yang disebabkan oleh ekstensi browser yang menyuntikkan elemen lalu dihancurkan saat proses _hydration_. SvelteKit akan memberi peringatan pada saat pengembangan jika hal ini terjadi
  - `%sveltekit.assets%` — [`paths.assets`](configuration#paths) jika ditentukan, atau path relatif ke [`paths.base`](configuration#paths)
  - `%sveltekit.nonce%` — sebuah [CSP](configuration#csp) nonce untuk link dan script yang dimasukkan secara manual jika digunakan
  - `%sveltekit.env.[NAME]%` — akan diganti saat proses render dengan environment variable `[NAME]`, yang harus dimulai dengan [`publicPrefix`](configuration#env) (biasanya `PUBLIC_`). Jika tidak cocok akan fallback ke `''`
- `error.html` adalah halaman yang di-render ketika semuanya gagal. Halaman ini dapat berisi placeholder berikut:
  - `%sveltekit.status%` — kode status HTTP yang dihasilkan
  - `%sveltekit.error.message%` — pesan kesalahan yang dihasilkan
- `hooks.client.js` berisi [hooks](hooks) client kamu
- `hooks.server.js` berisi [hooks](hooks) server kamu
- `service-worker.js` berisi [service worker](service-workers) kamu

(Kamu dapat memilih untuk menggunakan antara JavaScript dan TypeScript ketika kamu membuat proyek. Kamu dapat beralih antara JavaScript dan TypeScript di dokumentasi menggunakan toggle di bagian bawah halaman ini.)

Jika kamu menambahkan [Vitest](https://vitest.dev) ketika kamu membuat proyek, unit test kamu akan berada di direktori `src` dengan ekstensi `.test.js`.

### static

aset statis yang harus disajikan apa adanya, seperti `robots.txt` atau `favicon.png`, masuk ke sini.

### tests

Jika saat kamu membuat proyek lalu kamu menambahkan [Playwright](https://playwright.dev/) untuk melakukan pengujian browser, pengujian akan berada di direktori ini.

### package.json

`package.json` kamu harus mencakup `@sveltejs/kit`, `svelte` dan `vite` sebagai `devDependencies`.

When you create a project with `npm create svelte@latest`, you'll also notice that `package.json` includes `"type": "module"`. This means that `.js` files are interpreted as native JavaScript modules with `import` and `export` keywords. Legacy CommonJS files need a `.cjs` file extension.
Saat kamu membuat proyek dengan `npm create svelte@latest`, kamu akan melihat bahwa `package.json` mencakup `"type": "module"`. Ini berarti file `.js` akan diinterpretasikan sebagai native JavaScript modules dengan kata kunci `import` dan `export`. File CommonJS legacy membutuhkan ekstensi file `.cjs`.

### svelte.config.js

File ini berisi konfigurasi Svelte dan SvelteKit kamu [configuration](configuration).

### tsconfig.json

File ini (atau `jsconfig.json`, jika kamu lebih suka file `.js` yang type-checked daripada file `.ts`) adalah file konfigurasi TypeScript yang kamu tambahkan saat menjalankan `npm create svelte@latest`. 

Karena SvelteKit bergantung pada konfigurasi tertentu yang diatur dengan cara tertentu, SvelteKit akan menghasilkan file `.svelte-kit/tsconfig.json` yang konfigurasinya akan di-`extend` oleh konfigurasi kamu sendiri.

### vite.config.js

Proyek SvelteKit sebenarnya hanyalah proyek [Vite](https://vitejs.dev) yang menggunakan plugin [`@sveltejs/kit/vite`](modules#sveltejs-kit-vite) bersama dengan konfigurasi [Vite lainnya](https://vitejs.dev/config/).

## File-file lainnya

### .svelte-kit

Selama kamu mengembangkan dan membangun proyek kamu, SvelteKit akan menghasilkan file di direktori `.svelte-kit` (dapat dikonfigurasi sebagai [`outDir`](configuration#outdir)). Kamu dapat mengabaikan isinya, dan menghapusnya kapan saja, namun direktori tersebut akan dibuat kembali ketika kamu menjalakan `dev` atau `build`.

