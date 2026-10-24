<script>
	import '../app.css';
	import favicon from '#lib/assets/favicon.svg';
	import Nav from '../lib/components/Nav.svelte';
	import Footer from '../lib/components/Footer.svelte';

	/** @type {{children: import('svelte').Snippet}} */
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<Nav logo="/logo.png" />

<main>
	{@render children()}
</main>

<Footer logo="/logo.png" facebookUrl="https://www.facebook.com/1000Hills/" />