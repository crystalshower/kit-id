---
title: Routing
---

Di dalam SvelteKit, terdapat _filesystem-based router_. Rute dari aplikasi kamu — yaitu jalur URL yang dapat diakses pengguna yang didefinisikan oleh direktori dalam kode sumber kamu:

- `src/routes` adalah rute akar (root)
- `src/routes/about` membuat rute `/about`
- `src/routes/blog/[slug]` membuat rute dengan _parameter_, `slug`, yang dapat digunakan untuk memuat data secara dinamis ketika pengguna meminta halaman seperti `/blog/hello-world`

> Kamu dapat mengubah `src/routes` ke direktori yang berbeda dengan mengedit [konfigurasi proyek](configuration).

Setiap direktori rute berisi satu atau lebih _file rute_, yang dapat diidentifikasi dengan awalan `+`.

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

Jika fungsi `load` kamu hanya dapat berjalan di _server_ — misalnya, jika perlu mengambil data dari database atau kamu perlu mengakses [variabel lingkungan](modules#$env-static-private) seperti kunci API — maka kamu dapat mengubah nama `+page.js` menjadi `+page.server.js` dan mengubah tipe `PageLoad` menjadi `PageServerLoad`.

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
We can create a layout that only applies to pages below `/settings` (while inheriting the root layout with the top-level nav):

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

Just like `+page.svelte` loading data from `+page.js`, your `+layout.svelte` component can get data from a [`load`](load) function in `+layout.js`.

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

If a `+layout.js` exports [page options](page-options) — `prerender`, `ssr` and `csr` — they will be used as defaults for child pages.

Data returned from a layout's `load` function is also available to all its child pages:

```svelte
<!--- file: src/routes/settings/profile/+page.svelte --->
<script>
	/** @type {import('./$types').PageData} */
	export let data;

	console.log(data.sections); // [{ slug: 'profile', title: 'Profile' }, ...]
</script>
```

> Often, layout data is unchanged when navigating between pages. SvelteKit will intelligently rerun [`load`](load) functions when necessary.

### +layout.server.js

To run your layout's `load` function on the server, move it to `+layout.server.js`, and change the `LayoutLoad` type to `LayoutServerLoad`.

Like `+layout.js`, `+layout.server.js` can export [page options](page-options) — `prerender`, `ssr` and `csr`.

## +server

As well as pages, you can define routes with a `+server.js` file (sometimes referred to as an 'API route' or an 'endpoint'), which gives you full control over the response. Your `+server.js` file exports functions corresponding to HTTP verbs like `GET`, `POST`, `PATCH`, `PUT`, `DELETE`, `OPTIONS`, and `HEAD` that take a `RequestEvent` argument and return a [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) object.

For example we could create an `/api/random-number` route with a `GET` handler:

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

The first argument to `Response` can be a [`ReadableStream`](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream), making it possible to stream large amounts of data or create server-sent events (unless deploying to platforms that buffer responses, like AWS Lambda).

You can use the [`error`](modules#sveltejs-kit-error), [`redirect`](modules#sveltejs-kit-redirect) and [`json`](modules#sveltejs-kit-json) methods from `@sveltejs/kit` for convenience (but you don't have to).

If an error is thrown (either `error(...)` or an unexpected error), the response will be a JSON representation of the error or a fallback error page — which can be customised via `src/error.html` — depending on the `Accept` header. The [`+error.svelte`](#error) component will _not_ be rendered in this case. You can read more about error handling [here](errors).

> When creating an `OPTIONS` handler, note that Vite will inject `Access-Control-Allow-Origin` and `Access-Control-Allow-Methods` headers — these will not be present in production unless you add them.

### Receiving data

By exporting `POST`/`PUT`/`PATCH`/`DELETE`/`OPTIONS`/`HEAD` handlers, `+server.js` files can be used to create a complete API:

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

> In general, [form actions](form-actions) are a better way to submit data from the browser to the server.

> If a `GET` handler is exported, a `HEAD` request will return the `content-length` of the `GET` handler's response body.

### Fallback method handler

Exporting the `fallback` handler will match any unhandled request methods, including methods like `MOVE` which have no dedicated export from `+server.js`.

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

> For `HEAD` requests, the `GET` handler takes precedence over the `fallback` handler.

### Content negotiation

`+server.js` files can be placed in the same directory as `+page` files, allowing the same route to be either a page or an API endpoint. To determine which, SvelteKit applies the following rules:

- `PUT`/`PATCH`/`DELETE`/`OPTIONS` requests are always handled by `+server.js` since they do not apply to pages
- `GET`/`POST`/`HEAD` requests are treated as page requests if the `accept` header prioritises `text/html` (in other words, it's a browser page request), else they are handled by `+server.js`.
- Responses to `GET` requests will include a `Vary: Accept` header, so that proxies and browsers cache HTML and JSON responses separately.

## $types

Throughout the examples above, we've been importing types from a `$types.d.ts` file. This is a file SvelteKit creates for you in a hidden directory if you're using TypeScript (or JavaScript with JSDoc type annotations) to give you type safety when working with your root files.

For example, annotating `export let data` with `PageData` (or `LayoutData`, for a `+layout.svelte` file) tells TypeScript that the type of `data` is whatever was returned from `load`:

```svelte
<!--- file: src/routes/blog/[slug]/+page.svelte --->
<script>
	/** @type {import('./$types').PageData} */
	export let data;
</script>
```

In turn, annotating the `load` function with `PageLoad`, `PageServerLoad`, `LayoutLoad` or `LayoutServerLoad` (for `+page.js`, `+page.server.js`, `+layout.js` and `+layout.server.js` respectively) ensures that `params` and the return value are correctly typed.

If you're using VS Code or any IDE that supports the language server protocol and TypeScript plugins then you can omit these types _entirely_! Svelte's IDE tooling will insert the correct types for you, so you'll get type checking without writing them yourself. It also works with our command line tool `svelte-check`.

You can read more about omitting `$types` in our [blog post](https://svelte.dev/blog/zero-config-type-safety) about it.

## Other files

Any other files inside a route directory are ignored by SvelteKit. This means you can colocate components and utility modules with the routes that need them.

If components and modules are needed by multiple routes, it's a good idea to put them in [`$lib`](modules#$lib).

## Further reading

- [Tutorial: Routing](https://learn.svelte.dev/tutorial/pages)
- [Tutorial: API routes](https://learn.svelte.dev/tutorial/get-handlers)
- [Docs: Advanced routing](advanced-routing)
