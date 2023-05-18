<script>
	import { Section } from '@sveltejs/site-kit/components';
</script>

<Section --background="var(--sk-back-1)">
	<div class="grid" style="--columns: 3">
		<div>
			<h2>cepat</h2>
			<p>
				Didukung oleh <a target="_blank" rel="noreferrer" href="https://svelte.dev">Svelte</a> dan
				<a target="_blank" rel="noreferrer" href="https://vitejs.dev">Vite</a>, kecepatan tersemat di setiap sudut:
				persiapan cepat, pengembangan cepat, pembangunan cepat, muatan halaman cepat, navigasi cepat. 
				Sudahkah kami menyebut cepat?
			</p>
		</div>

		<div>
			<h2>seru</h2>
			<p>
				Tak ada lagi waktu yang terbuang untuk mencari konfigurasi bundler, routing, SSR, CSP, TypeScript,
				pengaturan deployment, dan semua hal membosankan lainnya. Berkreasilah dengan penuh sukacita.
			</p>
		</div>

		<div>
			<h2>fleksibel</h2>
			<p>
				SPA? MPA? SSR? SSG? Semua bisa dilakukan. SvelteKit memberikanmu alat yang tepat 
				untuk mencapai keberhasilan apapun yang sedang kamu bangun. 
				Dan dapat berjalan di mana saja JavaScript berjalan.
			</p>
		</div>
	</div>
</Section>

<style>
	p {
		font-size: var(--sk-text-m);
	}
</style>
