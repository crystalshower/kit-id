<script>
	import { Section } from '@sveltejs/site-kit/components';
</script>

<Section --background="var(--sk-back-1)">
	<div class="container">
		<div class="features">
			<h2><span>fitur? ada semua.</span></h2>
			<p class="wall">
				Gabungkan halaman <strong>prarender</strong> untuk kinerja maksimum dengan
				<strong>server side rendering</strong>
				untuk fleksibilitas maksimum. Jadikan aplikasimu menjadi
				<strong>PWA</strong>
				yang dirender oleh klien dengan satu baris kode, untuk seluruh halaman atau hanya satu halaman
				saja. Gunakan <strong>routing sisi klien</strong> yang mudah diakses dengan
				<strong>preloading</strong>
				otomatis untuk navigasi instan yang halus tanpa memuat ulang seluruh halaman (termasuk analitik
				dan semua sampah lainnya). Lindungi pengguna kamu dengan proteksi
				<strong>CSRF</strong>
				otomatis dan konfigurasi <strong>Content Security Policy</strong> yang mudah digunakan.
				Simpan rahasiamu dengan penanganan <strong>environment varible</strong> yang canggih.
				Tangani kesalahan dengan anggun dan <strong>aman</strong>. Muat data kamu langsung dari
				database, lalu hubungkan backend dengan frontend menggunakan data loading
				<strong>yang aman</strong>
				dan <strong>form actions</strong> bawaan yang dapat berfungsi tanpa JavaScript. Dapat
				<strong>berdampingan</strong>
				dengan framework routing sisi-klien lainnya di halaman yang sama. Tambahkan service worker untuk
				dukungan <strong>offline</strong>. Hasilkan halaman yang sesuai dengan
				<strong>AMP</strong>
				jika kamu benar-benar perlu. Bangun antarmuka pengguna yang kompleks dengan
				<strong>filesystem-based routes</strong>
				yang luar biasa kuat. Tata letak bertingkat? Tentu saja! Pelajari
				<strong>standar web</strong>
				yang bekerja di semua lingkungan. Integrasi dengan <strong>Tailwind</strong>,
				<strong>Playwright</strong>, <strong>Vitest</strong>, dan <strong>Storybook</strong>, serta
				apa pun yang kamu inginkan. Bangun <strong>library</strong> serta aplikasi.
				<strong>Deploy</strong> di mana saja dengan adapter.
			</p>

			<p>
				SvelteKit adalah sebuah framework yang
				<strong>menyertaimu</strong>, apapun yang kamu bangun.
			</p>
		</div>
	</div>
</Section>

<style>
	.container {
		display: flex;
		flex-direction: column;
	}

	.features {
		order: 1;
		text-align: justify;
	}

	h2 {
		float: right;
		width: 5.5em;
	}

	h2 span {
		display: block;
		text-align: right;
		width: 5em;
		margin: 0 0 0 auto;
	}

	.wall {
		margin: 0;
		font-size: var(--sk-text-xs);
	}

	p + p {
		margin-top: 1em;
	}

	p,
	strong {
		color: var(--sk-text-3);
	}

	@media (min-width: 600px) {
		h2 {
			margin-top: 0.5em;
			width: 10em;
		}

		h2 span {
			width: auto;
		}

		/* TODO remove the 62.5% hack so we can use rems in media queries */
		.container {
			--size: 40rem;
			display: block;
		}

		.features {
			padding: 2rem 0 2rem calc(0 * var(--size));
		}
	}

	@media (min-width: 900px) {
		.container {
			--size: 45rem;
		}

		h2 {
			margin-top: 0.1em;
		}

		p {
			font-size: var(--sk-text-s);
		}

		.features {
			padding-top: clamp(0px, calc(23.333vw - 210px), 70px);
		}
	}
</style>
