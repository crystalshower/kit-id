---
title: Routing
---

Di dalam SvelteKit, terdapat _filesystem-based router_. Rute dari aplikasi kamu — yaitu jalur URL yang dapat diakses pengguna yang didefinisikan oleh direktori dalam kode sumber kamu:

- `src/routes` adalah rute akar (root)
- `src/routes/about` membuat rute `/about`
- `src/routes/blog/[slug]` membuat rute dengan _parameter_, `slug`, yang dapat digunakan untuk memuat data secara dinamis ketika pengguna meminta halaman seperti `/blog/hello-world`

> Kamu dapat mengubah `src/routes` ke direktori yang berbeda dengan mengedit [konfigurasi proyek](configuration).

Setiap direktori rute berisi satu atau lebih _file rute_, yang dapat diidentifikasi dengan awalan `+`.

We'll introduce these files in a moment in more detail, but here are a few simple rules to help you remember how SvelteKit's routing works:

* All files can run on the server
* All files run on the client except `+server` files
* `+layout` and `+error` files apply to subdirectories as well as the directory they live in

## +page

### +page.svelte

Komponen `+page.svelte` mendefinisikan halaman aplikasi kamu. Secara default, halaman dirender di _server_ ([SSR](glossary#ssr)) untuk permintaan awal dan di _browser_ ([CSR](glossary#csr)) untuk navigasi selanjutnya.

```svelte
<!--- file: src/routes/+page.svelte --->
<h1>Selamat datang di situs saya</h1>
<a href="/tentang">Tentang situs saya</a>
```

```svelte
<!--- file: src/routes/tentang/+page.svelte --->
<h1>Tentang situs saya</h1>
<p>TODO...</p>
<a href="/">Beranda</a>
```

```svelte
<!--- file: src/routes/blog/[slug]/+page.svelte --->
<script>
	/** @type {import('./$types').PageData} */
	export let data;
</script>

<h1>{data.title}</h1>
<div>{@html data.content}</div>
```

> Perhatikan bahwa SvelteKit menggunakan elemen `<a>` untuk menavigasi antar rute, bukan komponen `<Link>` yang sering digunakan pada _framework_ lainnya.

### +page.js

Seringkali, sebuah halaman akan perlu memuat beberapa data sebelum dapat dirender (dalam hal ini, kita menambahkan modul `+page.js` yang mengekspor fungsi `load`):

```js
/// file: src/routes/blog/[slug]/+page.js
import { error } from '@sveltejs/kit';

/** @type {import('./$types').PageLoad} */
export function load({ params }) {
	if (params.slug === 'hello-world') {
		return {
			title: 'Hello world!',
			content: 'Selamat datang di blog kami. Lorem ipsum dolor sit amet...'
		};
	}

	error(404, 'Tidak ditemukan');
}
```
Fungsi ini berjalan bersamaan dengan `+page.svelte`, yang berarti berjalan di _server_ selama _server-side rendering_ dan di _browser_ selama navigasi _client-side_. Lihat [`load`](load) untuk detail lengkap dari API ini.

Selain `load`, `+page.js` dapat mengekspor nilai yang mengonfigurasi perilaku halaman:

- `export const prerender = true` atau `false` atau `'auto'`
- `export const ssr = true` atau `false`
- `export const csr = true` atau `false`

Kamu dapat menemukan informasi lebih lanjut tentang ini di [halaman opsi](page-options).

### +page.server.js

Jika kamu ingin menjalankan fungsi `load` di _server_ — misalnya, jika perlu mengambil data dari database atau kamu perlu mengakses [environment variable](modules#$env-static-private) seperti API key — maka kamu dapat mengubah nama `+page.js` menjadi `+page.server.js` dan mengubah tipe `PageLoad` menjadi `PageServerLoad`.

```js
/// file: src/routes/blog/[slug]/+page.server.js

// @filename: ambient.d.ts
declare global {
	const getPostFromDatabase: (slug: string) => {
		title: string;
		content: string;
	}
}

export {};

// @filename: index.js
// ---cut---
import { error } from '@sveltejs/kit';

/** @type {import('./$types').PageServerLoad} */
export async function load({ params }) {
	const post = await getPostFromDatabase(params.slug);

	if (post) {
		return post;
	}

	error(404, 'Not found');
}
```

Selama navigasi _client-side_, SvelteKit akan memuat data ini dari _server_, yang berarti nilai yang dikembalikan harus dapat diserialisasi menggunakan [devalue](https://github.com/rich-harris/devalue). Lihat [`load`](load) untuk detail lengkap dari API ini.

Seperti `+page.js`, `+page.server.js` dapat mengekspor [opsi halaman](page-options) — `prerender`, `ssr` dan `csr`.

`+page.server.js` juga dapat mengekspor _actions_. Jika `load` memungkinkan kamu bisa membaca data dari _server_, _actions_ memungkinkan kamu menulis data _ke_ server menggunakan elemen `<form>`. Untuk mempelajari cara menggunakannya, lihat bagian [form actions](form-actions).

## +error

Jika terjadi kesalahan selama `load`, SvelteKit akan merender halaman kesalahan default. Kamu dapat menyesuaikan halaman kesalahan ini berdasarkan rute dengan menambahkan file `+error.svelte`:

```svelte
<!--- file: src/routes/blog/[slug]/+error.svelte --->
<script>
	import { page } from '$app/stores';
</script>

<h1>{$page.status}: {$page.error.message}</h1>
```

SvelteKit akan mencari _error boundary_ terdekat — jika file di atasnya tidak ada, ia akan mencoba `src/routes/blog/+error.svelte` dan kemudian `src/routes/+error.svelte` sebelum merender halaman kesalahan default. Jika _itu_ gagal (atau jika kesalahan dilempar dari fungsi `load` dari `+layout` akar, yang berada 'di atas' `+error` akar), SvelteKit akan keluar dan merender halaman kesalahan statis cadangan, yang dapat kamu sesuaikan dengan membuat file `src/error.html`.

Jika kesalahan terjadi di dalam fungsi `load` di `+layout(.server).js`, _error boundary_ terdekat dalam pohon adalah file `+error.svelte` _di atas_ layout itu (bukan di sebelahnya).

Jika tidak ada rute yang ditemukan (404), `src/routes/+error.svelte` (atau halaman kesalahan default, jika file tersebut tidak ada) akan digunakan.

> `+error.svelte` _tidak_ digunakan ketika kesalahan terjadi di dalam [`handle`](hooks#server-hooks-handle) atau penanganan permintaan [+server.js](#server).

Kamu dapat membaca lebih lanjut tentang penanganan kesalahan [di sini](errors).

## +layout

Selama ini, kita telah memperlakukan halaman sebagai komponen yang sepenuhnya mandiri — saat berpindah halaman, komponen `+page.svelte` yang ada akan dihancurkan, dan digantikan dengan yang baru.

Tetapi dalam banyak aplikasi, ada elemen yang harus terlihat di _setiap_ halaman, seperti navigasi tingkat atas atau footer. Daripada mengulangnya di setiap `+page.svelte`, kita dapat meletakkannya di _layouts_.

### +layout.svelte

Untuk membuat layout yang berlaku untuk setiap halaman, buat file bernama `src/routes/+layout.svelte`. Layout default (yang digunakan SvelteKit jika kamu tidak membawa milikmu) terlihat seperti ini...

```html
<slot></slot>
```

...tetapi kita dapat menambahkan apa pun yang kita inginkan. Salah satu syaratnya adalah komponen tersebut harus mencakup `<slot>`. Sebagai contoh, mari tambahkan bilah navigasi:

```html
/// file: src/routes/+layout.svelte
<nav>
	<a href="/">Beranda</a>
	<a href="/tentang">Tentang</a>
	<a href="/pengaturan">Pengaturan</a>
</nav>

<slot></slot>
```

Ketika kita membuat halaman untuk `/`, `/tentang` dan `/pengaturan`...

```html
/// file: src/routes/+page.svelte
<h1>Beranda</h1>
```

```html
/// file: src/routes/tentang/+page.svelte
<h1>Tentang</h1>
```

```html
/// file: src/routes/pengaturan/+page.svelte
<h1>Pengaturan</h1>
```

...bilah navigasi akan selalu terlihat, dan mengklik antara tiga halaman hanya akan mengganti `<h1>`.

Layout juga dapat _ditumpuk_. Misalkan kita tidak hanya memiliki satu halaman `/pengaturan`, tetapi sebaliknya memiliki halaman tumpuk seperti `/pengaturan/profil` dan `/pengaturan/notifikasi` dengan submenu bersama (untuk contoh nyata, lihat [github.com/settings](https://github.com/settings)).

Kita dapat membuat Layout yang hanya berlaku untuk halaman di bawah `/pengaturan` (sambil mewarisi tata letak akar dengan navigasi tingkat atas):

```svelte
<!--- file: src/routes/settings/+layout.svelte --->
<script>
	/** @type {import('./$types').LayoutData} */
	export let data;
</script>

<h1>Pengaturan</h1>

<div class="submenu">
	{#each data.sections as section}
		<a href="/pengaturan/{section.slug}">{section.title}</a>
	{/each}
</div>

<slot></slot>
```

Kamu dapat melihat bagaimana `data` diisi dengan melihat contoh `+layout.js` di bagian berikutnya di bawah ini.

Secara default, setiap _layout_ mewarisi _layout_ di atasnya. Terkadang hal itu bukan yang kamu inginkan - dalam hal ini, [tata letak lanjutan](advanced-routing#advanced-layouts) dapat membantu kamu.

### +layout.js
Seperti `+page.svelte` yang memuat data dari `+page.js`, komponen `+layout.svelte` kamu mendapatkan data dari fungsi [`load`](load) di `+layout.js`.

```js
/// file: src/routes/settings/+layout.js
/** @type {import('./$types').LayoutLoad} */
export function load() {
	return {
		sections: [
			{ slug: 'profile', title: 'Profile' },
			{ slug: 'notifications', title: 'Notifications' }
		]
	};
}
```

Jika `+layout.js` mengekspor [opsi halaman](page-options) — `prerender`, `ssr` dan `csr` — mereka akan digunakan secara default untuk halaman turunannya.

Data yang dikembalikan dari fungsi `load` layout juga tersedia untuk semua halaman turunannya:

```svelte
<!--- file: src/routes/settings/profile/+page.svelte --->
<script>
	/** @type {import('./$types').PageData} */
	export let data;

	console.log(data.sections); // [{ slug: 'profile', title: 'Profile' }, ...]
</script>
```

> Sering kali, data _layout_ tidak berubah ketika menavigasi antar halaman. SvelteKit akan secara cerdas menjalankan kembali fungsi [`load`](load) bila diperlukan.

### +layout.server.js

Untuk menjalankan fungsi `load` _layout_ kamu di _server_, pindahkan ke `+layout.server.js`, dan ubah tipe `LayoutLoad` menjadi `LayoutServerLoad`.

Seperti `+layout.js`, `+layout.server.js` dapat mengekspor [opsi halaman](page-options) — `prerender`, `ssr` dan `csr`.

## +server

As well as pages, you can define routes with a `+server.js` file (sometimes referred to as an 'API route' or an 'endpoint'), which gives you full control over the response. Your `+server.js` file exports functions corresponding to HTTP verbs like `GET`, `POST`, `PATCH`, `PUT`, `DELETE`, `OPTIONS`, and `HEAD` that take a `RequestEvent` argument and return a [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) object.
Layaknya sebuah halaman (+page), kamu juga bisa menambahkan rute dengan file `+server.js` (kadang disebut 'rute API' atau 'endpoint'), yang memberikan kontrol penuh atas suatu _response_. File `+server.js` kamu mengekspor fungsi yang sesuai dengan kata kerja HTTP seperti `GET`, `POST`, `PATCH`, `PUT`, `DELETE`, `OPTIONS`, dan `HEAD` yang mengambil argumen `RequestEvent` dan mengembalikan objek [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response).

For example we could create an `/api/random-number` route with a `GET` handler:
Berikut adalah contoh membuat rute `/api/random-number` dengan handler `GET`:

```js
/// file: src/routes/api/random-number/+server.js
import { error } from '@sveltejs/kit';

/** @type {import('./$types').RequestHandler} */
export function GET({ url }) {
	const min = Number(url.searchParams.get('min') ?? '0');
	const max = Number(url.searchParams.get('max') ?? '1');

	const d = max - min;

	if (isNaN(d) || d < 0) {
		error(400, 'min and max must be numbers, and min must be less than max');
	}

	const random = min + Math.random() * d;

	return new Response(String(random));
}
```

Argumen pertama yang bisa diberikan ke `Response` bisa berupa [`ReadableStream`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream) yang dapat digunakan untuk streaming data dalam jumlah besar atau membuat server-sent events (kecuali saat dideploy ke platform yang mem-buffer respon, seperti AWS Lambda).

Kamu bisa menggunakan metode [`error`](modules#sveltejs-kit-error), [`redirect`](modules#sveltejs-kit-redirect) dan [`json`](modules#sveltejs-kit-json) dari `@sveltejs/kit` untuk kemudahan (tapi tidak harus kok).


Jika terjadi kesalahan (baik `error(...)` atau kesalahan yang tidak terduga), respon akan mengirim representasi JSON dari kesalahan atau halaman kesalahan cadangan — yang dapat disesuaikan melalui `src/error.html` — tergantung pada header `Accept`. Komponen [`+error.svelte`](#error) _tidak_ akan dirender dalam kasus ini. Kamu bisa membaca lebih lanjut tentang penanganan kesalahan [di sini](errors).

> Saat membuat handler `OPTIONS`, perhatikan bahwa Vite akan menyuntikkan header `Access-Control-Allow-Origin` dan `Access-Control-Allow-Methods`. Pada produksi, header tersebut tidak akan ada kecuali kamu yang menambahkannya.

### Menerima data

By exporting `POST`/`PUT`/`PATCH`/`DELETE`/`OPTIONS`/`HEAD` handlers, `+server.js` files can be used to create a complete API:
Dengan mengekspor handler `POST`/`PUT`/`PATCH`/`DELETE`/`OPTIONS`/`HEAD`, file `+server.js` bisa digunakan untuk membuat API yang lengkap:


```svelte
<!--- file: src/routes/add/+page.svelte --->
<script>
	let a = 0;
	let b = 0;
	let total = 0;

	async function add() {
		const response = await fetch('/api/add', {
			method: 'POST',
			body: JSON.stringify({ a, b }),
			headers: {
				'content-type': 'application/json'
			}
		});

		total = await response.json();
	}
</script>

<input type="number" bind:value={a}> +
<input type="number" bind:value={b}> =
{total}

<button on:click={add}>Calculate</button>
```

```js
/// file: src/routes/api/add/+server.js
import { json } from '@sveltejs/kit';

/** @type {import('./$types').RequestHandler} */
export async function POST({ request }) {
	const { a, b } = await request.json();
	return json(a + b);
}
```

> Umumnya, [form actions](form-actions) adalah cara yang lebih baik untuk mengirimkan data dari browser ke server.

> Jika handler `GET` diekspor, permintaan `HEAD` akan mengembalikan `content-length` dari _body_ respon handler `GET`.

### Fallback method handler

Mengekspor handler `fallback` akan mencocokkan semua metode permintaan yang tidak ditangani, termasuk metode seperti `MOVE` yang tidak memiliki ekspor khusus dari `+server.js`.

```js
// @errors: 7031
/// file: src/routes/api/add/+server.js
import { json, text } from '@sveltejs/kit';

export async function POST({ request }) {
	const { a, b } = await request.json();
	return json(a + b);
}

// This handler will respond to PUT, PATCH, DELETE, etc.
/** @type {import('./$types').RequestHandler} */
export async function fallback({ request }) {
	return text(`I caught your ${request.method} request!`);
}
```

> Untuk permintaan `HEAD`, handler `GET` lebih diutamakan daripada handler `fallback`.
### Negosiasi konten

file `+server.js` dapat ditempatkan di direktori yang sama dengan file `+page`. Yang mana dapat digunakan untuk membuat rute yang sama menjadi halaman atau _endpoint_ API. Untuk menentukan yang mana, SvelteKit menerapkan aturan berikut:

- Permintaan `PUT`/`PATCH`/`DELETE`/`OPTIONS` selalu ditangani oleh `+server.js` karena tidak berlaku untuk halaman
- Permintaan `GET`/`POST`/`HEAD` dianggap sebagai permintaan halaman jika header `accept` memprioritaskan `text/html` (dengan kata lain, itu adalah permintaan halaman browser), jika tidak, mereka ditangani oleh `+server.js`.
- Respon terhadap permintaan `GET` akan menyertakan header `Vary: Accept`, sehingga _proxies_ dan browser akan menyimpan _response_ HTML dan JSON secara terpisah.

## $types

Sepanjang contoh di atas, kita telah mengimpor tipe dari file `$types.d.ts`. File itu dibuat oleh SvelteKit untuk kamu di direktori tersembunyi jika kamu menggunakan TypeScript (atau JavaScript dengan anotasi tipe JSDoc) untuk memberikan keamanan tipe saat bekerja dengan file akar kamu.

Contohnya, menandai `export let data` dengan `PageData` (atau `LayoutData`, untuk file `+layout.svelte`) memberitahu TypeScript bahwa tipe dari `data` adalah apa pun yang dikembalikan dari `load`:

```svelte
<!--- file: src/routes/blog/[slug]/+page.svelte --->
<script>
	/** @type {import('./$types').PageData} */
	export let data;
</script>
```

Selanjutnya, menandai `export function load` dengan `PageLoad`, `PageServerLoad`, `LayoutLoad` atau `LayoutServerLoad` (untuk `+page.js`, `+page.server.js`, `+layout.js` dan `+layout.server.js` secara berturut-turut) memastikan bahwa `params` dan nilai yang dikembalikan memiliki tipe yang benar.

Kamu dapat membacanya lebih lanjut tentang cara menghilangkan `$types` di [postingan blog](https://svelte.dev/blog/zero-config-type-safety) kami tentang hal itu.

## Other files

File lain di dalam direktori rute akan diabaikan oleh SvelteKit. Dengan demikian, kamu dapat menempatkan komponen dan modul utilitas dengan rute yang membutuhkannya.

Jika komponen dan modul dibutuhkan oleh beberapa rute, sebaiknya letakkan di dalam [`$lib`](modules#$lib).

## Bacaan lebih lanjut

- [Tutorial: Routing](https://learn.svelte.dev/tutorial/pages)
- [Tutorial: API routes](https://learn.svelte.dev/tutorial/get-handlers)
- [Docs: Advanced routing](advanced-routing)
