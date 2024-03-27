---
title: Membuat proyek
---

Langkah yang paling mudah untuk membuat aplikasi SvelteKit adalah dengan menjalankan `npm create`:

```bash
npm create svelte@latest my-app
cd my-app
npm install
npm run dev
```

Perintah pertama akan membuat proyek baru di direktori `my-app` dan menanyakan apakah kamu ingin mengatur beberapa tooling dasar seperti TypeScript. Lihat [integrasi](./integrations) untuk acuan dalam menyiapkan alat tambahan. Perintah selanjutnya akan menginstal dependensi dan memulai server pada alamat [localhost:5173](http://localhost:5173).

Ada dua konsep dasar:

- Setiap halaman aplikasi kamu adalah sebuah komponen [Svelte](https://svelte.dev)
- Kamu membuat halaman dengan menambahkan file ke direktori `src/routes` dari proyek kamu. Halaman-halaman tersebut akan di-render di server sehingga ketika pengunjung pertama kali datang ke aplikasi kamu, akan dimuat secepat mungkin. Kemudian aplikasi di sisi klien akan mengambil alih

Cobalah untuk mengedit file tersebut untuk merasakan bagaimana semuanya bekerja.

## Pengaturan editor

Kami sarankan untuk menggunakan [Visual Studio Code (alias VS Code)](https://code.visualstudio.com/download) dengan [ekstensi Svelte](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode). [dukungan juga tersedia untuk editor lainnya](https://sveltesociety.dev/tools#editor-support).
