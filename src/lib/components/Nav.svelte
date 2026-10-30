<!--
  Nav
  The site header: logo, main links, Give button, and a menu toggle on
  small screens. Use it once in src/routes/+layout.svelte.

  <Nav />
  <Nav links={[{ label: 'About', href: '/about' }]} giveHref="/donate" />
-->
<script lang="ts">
	import { page } from '$app/state';
	import { afterNavigate } from '$app/navigation';
	import Button from './Button.svelte';

	interface NavLink {
		label: string;
		href: string;
	}

	interface Props {
		links?: NavLink[];
		giveHref?: string;
		logoSrc?: string;
	}

	let {
		links = [
			{ label: 'Why We Serve', href: '/why-we-serve' },
			{ label: 'Where We Serve', href: '/where-we-serve' },
			{ label: 'See Us in Action', href: '/see-us-in-action' },
			{ label: 'Contact', href: '/contact' }
		],
		giveHref = '/give',
		logoSrc = '/images/1000hills-logo.png'
	}: Props = $props();

	let open = $state(false);

	// Close the mobile menu after moving to another page.
	afterNavigate(() => {
		open = false;
	});

	function isActive(href: string) {
		const path = page.url.pathname;
		return path === href || path.startsWith(`${href}/`);
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape' && open) open = false;
	}
</script>

<svelte:window onkeydown={handleKeydown} />

<header class="site-header">
	<div class="inner">
		<a href="/" class="logo">
			<img src={logoSrc} alt="1000 Hills Ministry International, home" width="218" height="145" />
		</a>

		<nav aria-label="Main">
			<ul id="main-menu" class="links" class:open>
				{#each links as link (link.href)}
					<li>
						<a href={link.href} aria-current={isActive(link.href) ? 'page' : undefined}>
							{link.label}
						</a>
					</li>
				{/each}
			</ul>

			<div class="actions">
				<Button href={giveHref} size="s">Give</Button>
				<button
					type="button"
					class="menu-toggle"
					aria-controls="main-menu"
					aria-expanded={open}
					aria-label={open ? 'Close menu' : 'Open menu'}
					onclick={() => (open = !open)}
				>
					<svg viewBox="0 0 24 24" aria-hidden="true">
						{#if open}
							<path d="M6 6 L18 18 M18 6 L6 18" />
						{:else}
							<path d="M4 7 H20 M4 12 H20 M4 17 H20" />
						{/if}
					</svg>
				</button>
			</div>
		</nav>
	</div>
</header>

<style>
	.site-header {
		position: relative;
		z-index: 10;
		background: var(--color-surface);
		border-bottom: var(--border-width) solid var(--color-border);
	}

	.inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-m) var(--size-gutter);
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: var(--space-m);
	}

	.logo {
		display: block;
		flex: none;
	}
	.logo img {
		display: block;
		height: 60px;
		width: auto;
	}

	nav {
		display: flex;
		align-items: center;
		gap: var(--space-l);
	}

	.links {
		display: flex;
		align-items: center;
		gap: var(--space-l);
		margin: 0;
		padding: 0;
		list-style: none;
	}

	.links a {
		display: inline-block;
		padding-block: var(--space-xs);
		font-size: var(--font-size-s);
		font-weight: var(--font-weight-strong);
		color: var(--color-text-link);
		text-decoration: none;
		border-bottom: 2px solid transparent;
	}
	.links a:hover {
		color: var(--color-text-link-hover);
	}
	.links a[aria-current='page'] {
		color: var(--color-text);
		border-bottom-color: var(--color-hill-near);
	}
	.links a:focus-visible,
	.menu-toggle:focus-visible,
	.logo:focus-visible {
		outline: 3px solid var(--color-text-link);
		outline-offset: 3px;
		border-radius: var(--border-radius-s);
	}

	.actions {
		display: flex;
		align-items: center;
		gap: var(--space-xs);
	}

	.menu-toggle {
		display: none;
		width: 2.75rem;
		height: 2.75rem;
		padding: 0;
		align-items: center;
		justify-content: center;
		background: var(--color-surface);
		border: var(--border-width) solid var(--color-border);
		border-radius: var(--border-radius);
		cursor: pointer;
	}
	.menu-toggle svg {
		width: 22px;
		height: 22px;
		fill: none;
		stroke: var(--color-text);
		stroke-width: 2;
		stroke-linecap: round;
	}

	/* Tablet and phone: links move into a drop-down panel */
	@media (max-width: 900px) {
		.inner {
			padding-inline: var(--space-m);
		}

		.logo img {
			height: 44px;
		}

		.menu-toggle {
			display: inline-flex;
		}

		.links {
			display: none;
			position: absolute;
			top: 100%;
			left: 0;
			right: 0;
			flex-direction: column;
			align-items: stretch;
			gap: 0;
			padding: var(--space-xs) var(--space-m) var(--space-m);
			background: var(--color-surface);
			border-bottom: var(--border-width) solid var(--color-border);
			box-shadow: 0 12px 24px rgb(0 49 99 / 0.08);
		}
		.links.open {
			display: flex;
		}

		.links a {
			display: block;
			padding: var(--space-s) 0;
			font-size: var(--font-size-m);
			border-bottom: var(--border-width) solid var(--color-border);
		}
		.links li:last-child a {
			border-bottom-color: transparent;
		}
		.links a[aria-current='page'] {
			border-bottom-color: var(--color-border);
			box-shadow: inset 3px 0 0 var(--color-hill-near);
			padding-left: var(--space-s);
		}
	}
</style>
