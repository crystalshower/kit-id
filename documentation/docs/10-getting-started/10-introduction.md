---
title: Pengenalan
---

## Sebelum kita mulai
> Jika kamu baru menggunakan Svelte atau SvelteKit, kami sarankan untuk memeriksa [tutorial interaktif](https://learn.svelte.dev).
>
> Jika kamu mengalami kesulitan, jangan ragu untuk mencari bantuan [Discord chatroom](https://svelte.dev/chat) (Bahasa Inggris).

## Apa itu SvelteKit?

SvelteKit adalah sebuah framework untuk mengembangkan aplikasi web yang kuat dengan performa tinggi dan cepat menggunakan [Svelte](https://svelte.dev/). Jika kamu sudah mengenal React, maka SvelteKit mirip dengan Next. Jika kamu sudah mengenal Vue, maka SvelteKit  mirip dengan Nuxt.

Untuk mempelajari lebih lanjut tentang jenis aplikasi yang dapat kamu bangun dengan SvelteKit, lihat [FAQ](/docs/faq#what-can-i-make-with-sveltekit).

## Apa itu Svelte?

Secara singkat, Svelte adalah cara untuk menulis komponen antarmuka pengguna seperti menu navigasi, bagian komentar dan formulir kontak. Dimana pengguna dapat melihat dan melakukan interaksi di browser mereka. Kompiler Svelte mengonversi komponen kamu menjadi kode JavaScript yang dapat dijalankan untuk merender HTML sebagai struktur halaman dan CSS untuk mengatur tampilan halaman. Kamu tidak perlu mengenal Svelte untuk memahami panduan ini, namun pengetahuan tentang Svelte akan membantu. Jika kamu ingin mempelajari lebih lanjut, silakan cek [tutorial Svelte](https://svelte.dev/tutorial).

## Apa kelebihan SvelteKit dibandingkan dengan Svelte?

Svelte merender komponen antarmuka pengguna. Kamu dapat menyusun komponen-komponen tersebut dan merender seluruh halaman hanya dengan Svelte, namun kamu membutuhkan lebih dari sekedar Svelte untuk menulis aplikasi secara keseluruhan.

SvelteKit siap membantu kamu membangun aplikasi web dengan mengikuti praktik terbaik yang modern dan menyediakan solusi untuk tantangan umum pada saat pengembangan. SvelteKit menyediakan fungsi dasar seperti [router](glossary#routing) - yang memperbarui tampilan penggunaka ketika tautan diklik - hingga kemampuan yang lebih canggih. Daftar fitur yang luas termasuk [optimisasi build](https://vitejs.dev/guide/features.html#build-optimizations) untuk memuat kode yang seminimal mungkin yang diperlukan; [dukungan offline](service-workers); [preloading](link-options#data-sveltekit-preload-data) muat halaman sebelum pengguna tiba; [rendering yang dapat dikonfigurasi](page-options) untuk menangani bagian berbeda dari aplikasi kamu di server melalui [SSR](glossary#ssr), di browser melalui [client-side rendering](glossary#csr), atau pada saat build dengan [prerendering](glossary#prerendering); [optimisasi gambar](images); dan banyak lagi. Membangun aplikasi dengan semua praktik terbaik modern adalah hal yang sangat rumit, namun SvelteKit datang untuk mengatasi semua hal membosankan itu dan kamu dapat melanjutkan bagian kreatifnya saja.

SvelteKit mencerminkan perubahan pada kode kamu secara instan di browser untuk memberikan pengalaman pengembangan yang cepat dan kaya fitur dengan memanfaatkan [Vite](https://vitejs.dev/) dengan [plugin Svelte](https://github.com/sveltejs/vite-plugin-svelte) untuk melakukan [Hot Module Replacement (HMR)](https://github.com/sveltejs/vite-plugin-svelte/blob/main/docs/config.md#hot).
