# Terjemahan Dokumentasi SvelteKit
Proyek ini bertujuan untuk menyediakan terjemahan lengkap dari dokumentasi resmi SvelteKit ke dalam Bahasa Indonesia, sehingga lebih mudah diakses dan dipahami oleh para developer berbahasa Indonesia. 

## Status
| Dokumen | Terjemahan | Review | 
| --- | :---: | :---: |
| [Halaman Utama](https://kit.svelte.dev/) | ✅ | ❌ |
| [Introduction](https://kit.svelte.dev/docs/introduction) | ✅ | ❌ |
| [Creating a project](https://kit.svelte.dev/docs/creating-a-project) | ✅ | ❌ |
| [Project structure](https://kit.svelte.dev/docs/project-structure) | ✅ | ❌ |
| [Web standards](https://kit.svelte.dev/docs/web-standards) | ✅ | ❌ |
| [Routing](https://kit.svelte.dev/docs/routing) | 📝 | ❌ |
| [Loading Data](https://kit.svelte.dev/docs/load) | 📝 | ❌ |
| [Form Actions](https://kit.svelte.dev/docs/form-actions) | ❌ | ❌ |
| [Page Options](https://kit.svelte.dev/docs/page-options) | ❌ | ❌ |
| [State Management](https://kit.svelte.dev/docs/state-management) | ❌ | ❌ |
| [Building Your App](https://kit.svelte.dev/docs/building-your-app) | ❌ | ❌ |
| [Adapters](https://kit.svelte.dev/docs/adapters) | ❌ | ❌ |
| [Zero-config deployments](https://kit.svelte.dev/docs/adapter-auto) | ❌ | ❌ |
| [Node Server](https://kit.svelte.dev/docs/adapter-node) | ❌ | ❌ |
| [Static Site Generation](https://kit.svelte.dev/docs/adapter-static) | ❌ | ❌ |
| [Single-page apps](https://kit.svelte.dev/docs/single-page-apps) | ❌ | ❌ |
| [Cloudflare Pages](https://kit.svelte.dev/docs/adapter-cloudflare) | ❌ | ❌ |
| [Cloudflare Workers](https://kit.svelte.dev/docs/adapter-cloudflare-workers) | ❌ | ❌ |
| [Netlify](https://kit.svelte.dev/docs/adapter-netlify) | ❌ | ❌ |
| [Vercel](https://kit.svelte.dev/docs/adapter-vercel) | ❌ | ❌ |
| [Writing Adapter](https://kit.svelte.dev/docs/writing-adapters) | ❌ | ❌ |
| Many more... |  |  |

✅: Selesai diterjemahkan/review

❌: Belum diterjemahkan/review

📝: Sedang diterjemahkan/review

## Kontribusi
Saya sangat terbuka untuk kontribusi dari siapapun. Jika Anda ingin berkontribusi, silahkan fork repository ini dan lakukan perubahan yang Anda inginkan. Setelah itu, silahkan buat pull request ke repository ini. Saya akan melakukan review dan merge jika perlu.
## Peraturan Kontribusi
- Gunakan Bahasa Indonesia yang baik dan benar.
- Minimalisasi penggunaan terjemahan kecerdasan buatan (misal: Google Translate, ChatGPT, Bing Translator, dll).
- Gaya penulisan tidak harus sama dengan dokumentasi asli, namun harus tetap mudah dipahami.
- Nada bahasa tidak harus formal, namun harus tetap sopan.
- Jangan lupa untuk menterjemahkan komentar pada _code block_ jika ada.

---
[![Chat](https://img.shields.io/discord/457912077277855764?label=chat&logo=discord)](https://svelte.dev/chat)

# SvelteKit

Web development, streamlined. Read the [documentation](https://kit.svelte.dev/docs) to get started.

### Packages

| Package                                                                     | Changelog                                                     |
| --------------------------------------------------------------------------- | ------------------------------------------------------------- |
| [@sveltejs/kit](packages/kit)                                               | [Changelog](packages/kit/CHANGELOG.md)                        |
| [@sveltejs/adapter-auto](packages/adapter-auto)                             | [Changelog](packages/adapter-auto/CHANGELOG.md)               |
| [@sveltejs/adapter-cloudflare](packages/adapter-cloudflare)                 | [Changelog](packages/adapter-cloudflare/CHANGELOG.md)         |
| [@sveltejs/adapter-cloudflare-workers](packages/adapter-cloudflare-workers) | [Changelog](packages/adapter-cloudflare-workers/CHANGELOG.md) |
| [@sveltejs/adapter-netlify](packages/adapter-netlify)                       | [Changelog](packages/adapter-netlify/CHANGELOG.md)            |
| [@sveltejs/adapter-node](packages/adapter-node)                             | [Changelog](packages/adapter-node/CHANGELOG.md)               |
| [@sveltejs/adapter-static](packages/adapter-static)                         | [Changelog](packages/adapter-static/CHANGELOG.md)             |
| [@sveltejs/adapter-vercel](packages/adapter-vercel)                         | [Changelog](packages/adapter-vercel/CHANGELOG.md)             |
| [@sveltejs/amp](packages/amp)                                               | [Changelog](packages/amp/CHANGELOG.md)                        |
| [@sveltejs/enhanced-img](packages/enhanced-img)                             | [Changelog](packages/enhanced-img/CHANGELOG.md)               |
| [@sveltejs/package](packages/package)                                       | [Changelog](packages/package/CHANGELOG.md)                    |
| [create-svelte](packages/create-svelte)                                     | [Changelog](packages/create-svelte/CHANGELOG.md)              |
| [svelte-migrate](packages/migrate)                                          | [Changelog](packages/migrate/CHANGELOG.md)                    |

[Additional adapters](https://sveltesociety.dev/packages?category=sveltekit-adapters) are maintained by the community.

## Bug reporting

Please make sure the issue you're reporting involves SvelteKit. Many issues related to how a project builds originate from [Vite](https://vitejs.dev/), which is used to build a SvelteKit project. You can create a new Vite project with `npm create vite@latest` for client-side only repros and `npm create vite-extra@latest` for SSR or library repros.

If an issue originates from Vite, please report it in the [Vite issue tracker](https://github.com/vitejs/vite/issues).

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for information on how to develop SvelteKit locally.

## Supporting Svelte

Svelte is an MIT-licensed open source project with its ongoing development made possible entirely by fantastic volunteers. If you'd like to support their efforts, please consider:

- [Becoming a backer on Open Collective](https://opencollective.com/svelte).

Funds donated via Open Collective will be used for compensating expenses related to Svelte's development such as hosting costs. If sufficient donations are received, funds may also be used to support Svelte's development more directly.

## License

[MIT](https://github.com/sveltejs/kit/blob/main/LICENSE)
