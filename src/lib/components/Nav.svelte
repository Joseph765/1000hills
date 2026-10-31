<!--
  Nav
  The site header: logo, main links, Give button, and a menu toggle on
  small screens. Use it once in src/routes/+layout.svelte.

  <Nav />
  <Nav links={[{ label: 'About', href: '/about' }]} giveHref="https://…" />
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
			{ label: 'About', href: '/about' },
			{ label: 'Service', href: '/service' },
			{ label: 'Contact', href: '/contact' }
		],
		giveHref = 'https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU',
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
			<div class="menu-panel" class:open>
				<ul id="main-menu" class="links">
					{#each links as link (link.href)}
						<li>
							<a href={link.href} aria-current={isActive(link.href) ? 'page' : undefined}>
								{link.label}
							</a>
						</li>
					{/each}
				</ul>
			</div>

			<div class="actions">
				<Button href={giveHref} size="s" target="_blank" rel="noopener noreferrer">
					Give<span class="visually-hidden"> (opens in a new tab)</span>
				</Button>
				<button
					type="button"
					class="menu-toggle"
					class:open
					aria-controls="main-menu"
					aria-expanded={open}
					aria-label={open ? 'Close menu' : 'Open menu'}
					onclick={() => (open = !open)}
				>
					<span class="menu-icon" aria-hidden="true">
						<span class="bar bar-1"></span>
						<span class="bar bar-2"></span>
						<span class="bar bar-3"></span>
					</span>
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

	/* Desktop: the panel is a no-op wrapper so .links lays out as a normal
	   flex child of nav. It only becomes a positioned, animated box at the
	   900px breakpoint below. */
	.menu-panel {
		display: contents;
	}

	.links {
		display: flex;
		align-items: center;
		gap: var(--space-xl);
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
		text-decoration-thickness: 2px;
		text-underline-offset: 6px;
	}
	.links a:hover {
		color: var(--color-text-link-hover);
		text-decoration-line: underline;
	}
	.links a[aria-current='page'] {
		color: var(--color-text);
		text-decoration-line: underline;
		text-decoration-color: var(--color-hill-near);
	}
	.links a:focus-visible,
	.menu-toggle:focus-visible,
	.logo:focus-visible {
		outline: 3px solid var(--color-text-link);
		outline-offset: 3px;
		border-radius: var(--border-radius-s);
	}

	/* Read by screen readers, hidden visually */
	.visually-hidden {
		position: absolute;
		width: 1px;
		height: 1px;
		padding: 0;
		margin: -1px;
		overflow: hidden;
		clip: rect(0 0 0 0);
		white-space: nowrap;
		border: 0;
	}

	.actions {
		display: flex;
		align-items: center;
		gap: var(--space-xs);
	}

	/* Menu toggle: a navy-outlined square (not a faint white-on-white box)
	   so it reads clearly as a button, same outline → solid-navy pattern
	   as the secondary Button variant. It stays solid navy while open. */
	.menu-toggle {
		display: none;
		width: 2.75rem;
		height: 2.75rem;
		padding: 0;
		align-items: center;
		justify-content: center;
		background: var(--color-surface);
		border: var(--border-width-strong) solid var(--color-border-strong);
		border-radius: var(--border-radius);
		cursor: pointer;
		transition:
			background-color 150ms ease,
			border-color 150ms ease;
	}
	.menu-toggle:hover {
		background: var(--color-accent);
		border-color: var(--color-accent);
	}
	.menu-toggle.open {
		background: var(--color-accent);
		border-color: var(--color-accent);
	}
	.menu-toggle.open:hover {
		background: var(--color-accent-hover);
		border-color: var(--color-accent-hover);
	}

	/* The three bars morph into an X: the top and bottom bars meet in the
	   middle and rotate, the middle bar fades out. */
	.menu-icon {
		position: relative;
		width: 20px;
		height: 14px;
		flex: none;
	}
	.bar {
		position: absolute;
		left: 0;
		width: 100%;
		height: 2px;
		border-radius: 1px;
		background: var(--color-text);
		transition:
			top 220ms ease,
			transform 220ms ease,
			opacity 150ms ease,
			background-color 150ms ease;
	}
	.bar-1 {
		top: 0;
	}
	.bar-2 {
		top: 6px;
	}
	.bar-3 {
		top: 12px;
	}
	.menu-toggle:hover .bar,
	.menu-toggle.open .bar {
		background: var(--color-text-on-accent);
	}
	.menu-toggle.open:hover .bar {
		background: var(--color-text-on-accent-hover);
	}
	.menu-toggle.open .bar-1 {
		top: 6px;
		transform: rotate(45deg);
	}
	.menu-toggle.open .bar-2 {
		opacity: 0;
	}
	.menu-toggle.open .bar-3 {
		top: 6px;
		transform: rotate(-45deg);
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

		/* The panel owns position + the open/close animation; .links just
		   lays out the list inside it. Collapsed via max-height (not
		   display:none) so the height change can transition. visibility
		   is what actually keeps the closed links out of the tab order
		   and unclickable; it's scoped to this media query only — an
		   `inert` attribute on the element would apply even on desktop,
		   where the panel is just a display:contents wrapper, and would
		   silently block the nav links from being clicked at all. */
		.menu-panel {
			display: block;
			position: absolute;
			top: 100%;
			left: 0;
			right: 0;
			overflow: hidden;
			max-height: 0;
			opacity: 0;
			visibility: hidden;
			background: var(--color-surface);
			border-bottom: var(--border-width) solid var(--color-border);
			box-shadow: 0 12px 24px rgb(0 49 99 / 0.08);
			transition:
				max-height 220ms ease,
				opacity 180ms ease,
				visibility 220ms;
		}
		.menu-panel.open {
			max-height: 24rem;
			opacity: 1;
			visibility: visible;
		}

		.links {
			display: flex;
			flex-direction: column;
			align-items: stretch;
			gap: 0;
			padding: var(--space-xs) var(--space-m) var(--space-m);
			transform: translateY(-8px);
			transition: transform 220ms ease;
		}
		.menu-panel.open .links {
			transform: translateY(0);
		}

		.links li + li {
			border-top: var(--border-width) solid var(--color-border);
		}

		.links a {
			display: block;
			padding: var(--space-s) 0;
			font-size: var(--font-size-m);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.menu-panel,
		.links,
		.bar {
			transition: none;
		}
	}
</style>
