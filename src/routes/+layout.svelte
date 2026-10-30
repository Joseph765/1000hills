<script lang="ts">
	import '#lib/styles/tokens.css';
	import Nav from '#lib/components/Nav.svelte';

	let { children } = $props();
</script>

<a class="skip-link" href="#main">Skip to content</a>

<Nav />

<main id="main">
	{@render children()}
</main>

<style>
	.skip-link {
		position: absolute;
		left: var(--space-m);
		top: -100px;
		z-index: 100;
		padding: var(--space-s) var(--space-m);
		background: var(--color-accent);
		color: var(--color-text-on-accent);
		font-weight: var(--font-weight-active);
		border-radius: var(--border-radius);
		text-decoration: none;
	}
	.skip-link:focus {
		top: var(--space-m);
	}
</style>
