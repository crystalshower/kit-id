---
title: Pengenalan
---

## Sebelum kita mulai
> Jika kamu baru menggunakan Svelte atau SvelteKit, kami sarankan untuk memeriksa [tutorial interaktif](https://learn.svelte.dev).
>
> Jika kamu mengalami kesulitan, jangan ragu untuk mencari bantuan [Discord chatroom](https://svelte.dev/chat) (Bahasa Inggris).

## Apa itu SvelteKit?

SvelteKit adalah sebuah framework untuk mengembangkan aplikasi web yang kuat dengan performa tinggi dan cepat menggunakan [Svelte](https://svelte.dev/). Jika kamu sudah mengenal React, maka SvelteKit mirip dengan Next. Jika kamu sudah mengenal Vue, maka SvelteKit  mirip dengan Nuxt.

## Apa itu Svelte?

Secara singkat, Svelte adalah cara untuk menulis komponen antarmuka pengguna seperti menu navigasi, bagian komentar dan formulir kontak. Dimana pengguna dapat melihat dan melakukan interaksi di browser mereka. Kompiler Svelte mengonversi komponen kamu menjadi kode JavaScript yang dapat dijalankan untuk merender HTML sebagai struktur halaman dan CSS untuk mengatur tampilan halaman. Kamu tidak perlu mengenal Svelte untuk memahami panduan ini, namun pengetahuan tentang Svelte akan membantu. Jika kamu ingin mempelajari lebih lanjut, silakan cek [tutorial Svelte](https://svelte.dev/tutorial).

## Apa kelebihan SvelteKit dibandingkan dengan Svelte?

Svelte merender komponen antarmuka pengguna. Kamu dapat menyusun komponen-komponen tersebut dan merender seluruh halaman hanya dengan Svelte, namun kamu membutuhkan lebih dari sekedar Svelte untuk menulis aplikasi secara keseluruhan.

SvelteKit menyediakan fungsi dasar seperti [router](glossary#routing) - yang memperbarui UI ketika tautan diklik - dan [server-side rendering (SSR)](glossary#ssr). Namun selain itu, membangun aplikasi dengan semua langkah terbaik yang modern sangatlah rumit. Langkah-langkah tersebut meliputi [build optimizations](https://vitejs.dev/guide/features.html#build-optimizations), sehingga kamu hanya memuat kode yang diperlukan secara minimal; [dukungan offline](service-workers); [preloading](link-options#data-sveltekit-preload-data) halaman sebelum pengguna memulai navigasi; [pengaturan render](page-options) yang dapat kamu konfigurasi untuk merender bagian-bagian yang berbeda dari aplikasi kamu di server dengan [SSR](glossary#ssr), di browser [client-side rendering](glossary#csr), atau pada waktu pembangunan dengan [prerendering](glossary#prerendering); dan banyak hal lainnya. SvelteKit melakukan semua hal membosankan tersebut untukmu sehingga kamu dapat fokus pada bagian kreatif saja.

SvelteKit mencerminkan perubahan pada kode kamu secara instan di browser untuk memberikan pengalaman pengembangan yang cepat dan kaya fitur dengan memanfaatkan [Vite](https://vitejs.dev/) dengan [plugin Svelte](https://github.com/sveltejs/vite-plugin-svelte) untuk melakukan [Hot Module Replacement (HMR)](https://github.com/sveltejs/vite-plugin-svelte/blob/main/docs/config.md#hot).
