---
title: Standar Web
---

Sepanjang dokumentasi ini, kamu akan melihat referensi ke [Web APIs](https://developer.mozilla.org/en-US/docs/Web/API) standar yang SvelteKit bangun di atasnya. Daripada membuatnya lagi, kami _menggunakan platform_, yang berarti keterampilan pengembangan web yang kamu miliki saat ini dapat diterapkan ke SvelteKit. Sebaliknya, waktu yang dihabiskan untuk mempelajari SvelteKit akan membantu kamu menjadi pengembang web yang lebih baik di tempat lain.

API-API ini tersedia pada semua browser modern dan non-browser environments seperti Cloudflare Workers, Deno dan Vercel Edge Functions. Selama tahap pengembangan dan saat menggunakan [adapters](adapters) untuk environment yang berbasis Node (termasuk AWS Lambda), jika diperlukan, polyfills menyediakannya.(Namun untuk saat ini, hanya Node yang terus berkembang pesat saat ini mengenai standar web).

Secara khusus, kamu akan menjadi nyaman dengan hal-hal berikut:

## Fetch APIs

SvelteKit menggunakan [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/fetch) untuk mendapatkan data dari jaringan. Hal ini
tersedia di [hooks](hooks) dan [server routes](routing#server) serta di browser.

> Sebuah versi khusus dari `fetch` tersedia pada fungsi [`load`](load), [server hooks](hooks#server-hooks), dan [API routes](routing#server) untuk memanggil endpoint secara langsung selama server-side rendering tanpa melakukan panggilan HTTP, sambil mempertahankan kredensial. Untuk melakukan fetch dengan kredensial dalam kode server-side di luar `load`, kamu harus secara eksplisit melewatkan header `cookie` dan/atau `authorization`. Hal ini juga memungkinkan kamu untuk membuat permintaan relatif, sedangkan server-side `fetch` biasanya membutuhkan URL yang sepenuhnya terkualifikasi.

Selain `fetch` itu sendiri, [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) mencakup antarmuka berikut:

### Request

Sebuah instance dari [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) dapat diakses di [hooks](hooks) dan [server routes](routing#server) sebagai `event.request`. Request berisi metode yang berguna seperti `request.json()` dan `request.formData()` untuk mendapatkan data yang dikirim ke endpoint.

### Response

Sebuah instance dari [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) dikembalikan dari `await fetch(...)` dan handler di file `+server.js`. Pada dasarnya, aplikasi SvelteKit adalah sebuah mesin untuk mengubah `Request` menjadi `Response`.

### Headers

The [`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers) interface allows you to read incoming `request.headers` and set outgoing `response.headers`. For example, you can get the `request.headers` as shown below, and use the [`json` convenience function](modules#sveltejs-kit-json) to send modified `response.headers`:

```js
// @errors: 2461
/// file: src/routes/what-is-my-user-agent/+server.js
import { json } from '@sveltejs/kit';

/** @type {import('./$types').RequestHandler} */
export function GET({ request }) {
	// log all headers
	console.log(...request.headers);

	// create a JSON Response using a header we received
	return json({
		// retrieve a specific header
		userAgent: request.headers.get('user-agent')
	}, {
		// set a header on the response
		headers: { 'x-custom-header': 'potato' }
	});
}
```

## FormData

Ketika berurusan dengan pengiriman form asli HTML, kamu akan bekerja dengan objek [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData).

```js
// @errors: 2461
/// file: src/routes/hello/+server.js
import { json } from '@sveltejs/kit';

/** @type {import('./$types').RequestHandler} */
export async function POST(event) {
	const body = await event.request.formData();

	// log semua field
	console.log([...body]);

	return json({
		// dapatkan nilai field tertentu
		name: body.get('name') ?? 'world'
	});
}
```

## Stream APIs

Sebagian besar waktu, endpoint kamu akan mengembalikan data yang lengkap, seperti pada contoh `userAgent` di atas. Dan terkadang, kamu mungkin perlu mengembalikan respons yang terlalu besar untuk dimuat dalam satu memori sekaligus atau dikirimkan dalam potongan. Maka untuk ini platform menyediakan [streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API) — [ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream), [WritableStream](https://developer.mozilla.org/en-US/docs/Web/API/WritableStream) dan [TransformStream](https://developer.mozilla.org/en-US/docs/Web/API/TransformStream).

## URL APIs

URL direpresentasikan oleh interface [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL) yang mencakup properti berguna seperti `origin` dan `pathname` (dan di browser `hash`). Interface ini muncul di berbagai tempat, seperti `event.url` di [hooks](hooks) dan [server routes](routing#server), [`$page.url`](modules#$app-stores) di [pages](routing#page), `from` dan `to` di [`beforeNavigate` dan `afterNavigate`](modules#$app-navigation) dan seterusnya.

### URLSearchParams

Dimanapun kamu menemukan URL, kamu dapat mengakses parameter query melalui `url.searchParams` yang merupakan instance dari [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams):

```js
// @filename: ambient.d.ts
declare global {
	const url: URL;
}

export {};

// @filename: index.js
// ---cut---
const foo = url.searchParams.get('foo');
```

## Web Crypto

[Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) tersedia melalui global `crypto`. Web Crypto API digunakan secara internal untuk header [Content Security Policy](configuration#csp). Tetapi kamu juga dapat menggunakannya untuk hal-hal seperti menghasilkan UUID:

```js
const uuid = crypto.randomUUID();
```
