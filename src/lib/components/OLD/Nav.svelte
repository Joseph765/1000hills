<script lang="ts">
	import { page } from '$app/state';
	import { slide } from 'svelte/transition';

	type NavLink = { label: string; href: string };

	type Props = {
		logo: string;
		logoAlt?: string;
		links?: NavLink[];
		donateHref?: string;
		phone?: string;
	};

	let {
		logo,
		logoAlt = '1000 Hills Ministry International',
		links = [
			{ label: 'About', href: '/about' },
			{ label: 'Services', href: '/services' },
			{ label: 'Contact', href: '/contact' }
		],
		donateHref = 'https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU',
		phone = '281-787-8275'
	}: Props = $props();

	let open = $state(false);
	let scrollY = $state(0);
	let scrolled = $derived(scrollY > 8);

	const telHref = $derived(`tel:+1${phone.replace(/\D/g, '')}`);

	function isActive(href: string) {
		const path = page.url.pathname;
		return href === '/' ? path === '/' : path === href || path.startsWith(href + '/');
	}

	// Close the mobile menu whenever the route changes
	$effect(() => {
		page.url.pathname;
		open = false;
	});

	// Stop the page behind the open mobile menu from scrolling
	$effect(() => {
		document.body.style.overflow = open ? 'hidden' : '';
		return () => {
			document.body.style.overflow = '';
		};
	});

	function onKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape') open = false;
	}
</script>

<svelte:window bind:scrollY onkeydown={onKeydown} />

<header class="site-header" class:scrolled class:open>
	<div class="bar">
		<a class="brand" href="/" aria-label="1000 Hills Ministry home">
			<img src={logo} alt={logoAlt} />
		</a>

		<nav class="desktop-nav" aria-label="Main">
			<ul>
				{#each links as link (link.href)}
					<li>
						<a href={link.href} aria-current={isActive(link.href) ? 'page' : undefined}>
							{link.label}
						</a>
					</li>
				{/each}
			</ul>
		</nav>

		<div class="actions">
			<a class="donate" href={donateHref} target="_blank">Donate</a>

			<button
				class="menu-toggle"
				type="button"
				aria-expanded={open}
				aria-controls="mobile-menu"
				onclick={() => (open = !open)}
			>
				<span class="sr-only">{open ? 'Close menu' : 'Open menu'}</span>
				<span class="bars" aria-hidden="true"><span></span><span></span><span></span></span>
			</button>
		</div>
	</div>

	{#if open}
		<nav
			id="mobile-menu"
			class="mobile-menu"
			aria-label="Main"
			transition:slide={{ duration: 220 }}
		>
			<ul>
				{#each links as link (link.href)}
					<li>
						<a href={link.href} aria-current={isActive(link.href) ? 'page' : undefined}>
							{link.label}
						</a>
					</li>
				{/each}
			</ul>

			{#if phone}
				<p class="mobile-phone">Call us at <a href={telHref}>{phone}</a></p>
			{/if}
		</nav>
	{/if}
</header>

<style>
	.site-header {
		--nav-navy: #0e1e3c;
		--nav-blue: #1f3f75;
		--nav-accent: #c85f1c;
		--nav-accent-hover: #a94d14;
		--nav-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--nav-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);
		--nav-height: 72px;

		position: sticky;
		top: 0;
		z-index: 50;
		background: #fff;
		transition: box-shadow 0.2s ease;
	}

	.site-header,
	.site-header *,
	.site-header *::before,
	.site-header *::after {
		box-sizing: border-box;
	}

	.site-header.scrolled,
	.site-header.open {
		box-shadow: 0 2px 16px rgb(14 30 60 / 0.12);
	}

	.bar {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 1rem;
		max-width: 1200px;
		height: var(--nav-height);
		margin: 0 auto;
		padding: 0 1.25rem;
	}

	.brand {
		display: flex;
		align-items: center;
		height: 100%;
		padding: 8px 0;
	}

	.brand img {
		display: block;
		height: 100%;
		width: auto;
	}

	/* ---------- Desktop links (hidden on phones) ---------- */
	.desktop-nav {
		display: none;
	}

	.desktop-nav ul,
	.mobile-menu ul {
		margin: 0;
		padding: 0;
		list-style: none;
	}

	.desktop-nav ul {
		display: flex;
		gap: clamp(1.5rem, 3vw, 2.5rem);
	}

	.desktop-nav a {
		position: relative;
		padding: 0.5rem 0;
		font-family: var(--nav-display-font);
		font-size: 1.05rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		color: var(--nav-navy);
	}

	.desktop-nav a::after {
		content: '';
		position: absolute;
		left: 0;
		right: 0;
		bottom: 0;
		height: 2px;
		background: var(--nav-accent);
		transform: scaleX(0);
		transform-origin: left;
		transition: transform 0.2s ease;
	}

	.desktop-nav a[aria-current='page']::after {
		transform: scaleX(1);
	}

	.actions {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	.donate {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		border-radius: 4px;
		background: var(--nav-accent);
		color: #fff;
		font-family: var(--nav-display-font);
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		transition: background-color 0.2s ease;
	}

	.donate {
		min-height: 40px;
		padding: 0.5rem 1rem;
		font-size: 0.95rem;
	}

	/* ---------- Menu button ---------- */
	.menu-toggle {
		display: grid;
		place-items: center;
		width: 44px;
		height: 44px;
		padding: 0;
		border: 0;
		border-radius: 4px;
		background: transparent;
		color: var(--nav-navy);
		cursor: pointer;
	}

	.bars {
		position: relative;
		display: block;
		width: 24px;
		height: 18px;
	}

	.bars span {
		position: absolute;
		left: 0;
		width: 100%;
		height: 2px;
		border-radius: 2px;
		background: currentColor;
		transition:
			transform 0.2s ease,
			opacity 0.2s ease,
			top 0.2s ease;
	}

	.bars span:nth-child(1) { top: 0; }
	.bars span:nth-child(2) { top: 8px; }
	.bars span:nth-child(3) { top: 16px; }

	/* Bars turn into an X when open */
	.open .bars span:nth-child(1) { top: 8px; transform: rotate(45deg); }
	.open .bars span:nth-child(2) { opacity: 0; }
	.open .bars span:nth-child(3) { top: 8px; transform: rotate(-45deg); }

	/* ---------- Mobile menu panel ---------- */
	.mobile-menu {
		position: absolute;
		top: 100%;
		left: 0;
		right: 0;
		height: calc(100svh - var(--nav-height));
		overflow-y: auto;
		padding: 1rem 1.25rem 2rem;
		background: #fff;
		border-top: 1px solid rgb(14 30 60 / 0.08);
	}

	.mobile-menu li + li {
		border-top: 1px solid rgb(14 30 60 / 0.1);
	}

	.mobile-menu ul a {
		display: block;
		padding: 1rem 0;
		font-family: var(--nav-display-font);
		font-size: 1.6rem;
		font-weight: 600;
		letter-spacing: 0.03em;
		text-transform: uppercase;
		text-decoration: none;
		color: var(--nav-navy);
	}

	.mobile-menu ul a[aria-current='page'] {
		color: var(--nav-accent);
	}


	.mobile-phone {
		margin: 2rem 0 0;
		font-family: var(--nav-body-font);
		font-size: 1rem;
		color: rgb(14 30 60 / 0.75);
		text-align: center;
	}

	.mobile-phone a {
		color: var(--nav-blue);
		font-weight: 600;
	}

	.donate:focus-visible,
	.menu-toggle:focus-visible,
	.brand:focus-visible,
	.site-header nav a:focus-visible {
		outline: 3px solid var(--nav-accent);
		outline-offset: 3px;
	}

	.sr-only {
		position: absolute;
		width: 1px;
		height: 1px;
		margin: -1px;
		padding: 0;
		overflow: hidden;
		clip: rect(0 0 0 0);
		white-space: nowrap;
		border: 0;
	}

	@media (hover: hover) {
		.desktop-nav a:hover::after {
			transform: scaleX(1);
		}
		.donate:hover {
			background: var(--nav-accent-hover);
		}
	}

	/* ---------- Desktop ---------- */
	@media (min-width: 900px) {
		.site-header {
			--nav-height: 84px;
		}

		.bar {
			padding: 0 2rem;
		}

		.desktop-nav {
			display: block;
		}

		.menu-toggle,
		.mobile-menu {
			display: none;
		}

		.donate {
			min-height: 44px;
			padding: 0.6rem 1.4rem;
			font-size: 1.05rem;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.site-header,
		.desktop-nav a::after,
		.bars span,
		.donate {
			transition: none;
		}
	}
</style>