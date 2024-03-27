<script>
	import { Blurb } from '@sveltejs/site-kit/home';
</script>

<Blurb --background="var(--sk-back-1)">
	<div slot="one">
		<h2>cepat</h2>
		<p>
			Ditenagai oleh <a target="_blank" rel="noreferrer" href="https://svelte.dev">Svelte</a> dan
			<a target="_blank" rel="noreferrer" href="https://vitejs.dev">Vite</a>, kecepatan sudah tertanam di
			setiap celah: setup cepat, pengembangan cepat, build cepat, pemuatan halaman cepat, navigasi cepat. 
			Apakah kami menyebutkan ini dengan cepat?
		</p>
	</div>

	<div slot="two">
		<h2>menyenangkan</h2>
		<p>
			Tidak ada lagi hari-hari terbuang untuk mengatur konfigurasi bundler, routing, SSR, CSP, TypeScript,
			pengaturan deployment dan semua hal membosankan lainnya. Mengodinglah dengan senang hati.
		</p>
	</div>

	<div slot="three">
		<h2>fleksibel</h2>
		<p>
			SPA? MPA? SSR? SSG? Cek. SvelteKit memberikan alat yang kamu butuhkan untuk 
			berhasil dalam apa pun yang kamu bangun. Dan berjalan di mana pun JavaScript berjalan.
		</p>
	</div>
</Blurb>

<style>
	p {
		font-size: var(--sk-text-m);
	}
</style>
