<script>
	import { Section } from '@sveltejs/site-kit/components';
	import Video from './Video.svelte';
</script>

<Section --background="var(--background-1)">
	<p class="definition">
		<em>/ˈsvɛlt/</em> <span class="adjective">adjective</span>
		<span class="description">tipis tapi menarik, anggun dan bergaya.</span>
	</p>

	<div class="grid" style="--columns: 3">
		<div class="video-container">
			<Video />

			<p>
				<a
					target="_blank"
					rel="noreferrer"
					href="https://www.offerzen.com/community/svelte-origins-documentary"
				>
					Tonton dokumenter Svelte Origins secara penuh.
				</a>
			</p>
		</div>

		<div>
			<p>
				SvelteKit dibangun di atas Svelte, sebuah framework antarmuka menggunakan compiler yang
				bisa kamu gunakan untuk menulis komponen yang sangat singkat dan bekerja dengan minimal di browser
				menggunakan bahasa yang sudah kamu kenal seperti HTML, CSS, dan JavaScript. <strong
					>Seperti surat cinta untuk pengembangan web.</strong
				>
			</p>

			<p>
				Jangan hanya percaya pada kata-kata kami. Para developer juga menempatkan Svelte sebagai
				framework
				<a
					target="_blank"
					rel="noreferrer"
					href="https://insights.stackoverflow.com/survey/2021#section-most-loved-dreaded-and-wanted-web-frameworks"
				>
					paling
				</a>
				<a
					target="_blank"
					rel="noreferrer"
					href="https://tsh.io/state-of-frontend/#which-of-the-following-frameworks-would-you-like-to-learn-in-the-future"
				>
					menyenangkan
				</a>
				<a
					target="_blank"
					rel="noreferrer"
					href="https://2021.stateofjs.com/en-US/libraries/front-end-frameworks/"
				>
					untuk
				</a>

				<a
					target="_blank"
					rel="noreferrer"
					href="https://twitter.com/Rich_Harris/status/1589675637195042817"
				>
					digunakan</a
				>.
			</p>
		</div>
	</div>
</Section>

<style>
	p {
		font-size: var(--sk-text-s);
	}

	.definition {
		font-size: var(--sk-text-l);
	}

	.definition em {
		font-style: normal;
		font-size: var(--sk-text-xxl);
		color: var(--sk-theme-1);
	}

	.definition .adjective {
		position: relative;
		top: -0.15em;
		padding: 0.25em 0.5em 0.1em;
		line-height: 1;
		margin: 0 1em;
		text-transform: uppercase;
		font-size: var(--sk-text-s);
		background: var(--sk-text-2);
		border-radius: var(--sk-border-radius);
		color: var(--sk-back-2);
	}

	.definition .description {
		display: block;
	}

	@media (min-width: 900px) {
		.definition .description {
			display: inline;
		}

		.video-container {
			order: 2;
			grid-column: 2 / 4;
		}
	}
</style>
