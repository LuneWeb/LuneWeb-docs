<script lang="ts">
	import { base } from '$app/paths';
	import Document from '$lib/components/doc_page.svelte';
	import VerticalNavbar from '$lib/components/vertical_navbar.svelte';

	let { data } = $props();

	let pages = $derived(Object.keys(data.document.Pages));
	let elementsPromise = $derived(data.page.json());
</script>

<main>
	<VerticalNavbar>
		{#each pages as pageName}
			{#if pageName === data.pageName}
				<a class="section current" href="{base}/{data.document.Path}/{pageName}">>{pageName}</a>
			{:else}
				<a class="section" href="{base}/{data.document.Path}/{pageName}">{pageName}</a>
			{/if}
		{/each}

		<style>
			.section {
				text-align: start;
				font-weight: bolder;
				font-size: 1.2rem;
				padding: 0.1rem;
				transition: 200ms;
				width: 100%;
				color: blue;
				background-color: rgba(0, 0, 0, 0.1);
				text-decoration: underline;
				border-color: transparent;
				cursor: pointer;
			}

			.section.current {
				background-color: rgba(0, 0, 0, 0.2);
				cursor: default;
			}

			.section:hover {
				background-color: rgba(0, 0, 255, 0.2);
				transform: translateX(-1.5px);
			}
		</style>
	</VerticalNavbar>

	{#await elementsPromise then elements}
		<Document {elements}></Document>
	{:catch err}
		<h1>{err}</h1>
	{/await}
</main>

<style>
	main {
		display: flex;
	}

	@media (max-width: 600px) {
		main {
			display: block;
		}
	}

	@keyframes -global-popup {
		0% {
			transform: translateY(5px);
			opacity: 0;
		}

		80% {
			transform: translateY(-1px);
		}
	}
</style>
