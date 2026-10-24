<!--
  Footer.svelte — site footer for 1000 Hills Ministry International
  Place in: src/lib/components/Footer.svelte and render it in src/routes/+layout.svelte after the page content.
  Svelte 5 (runes). Email and Facebook only appear once you pass them.
-->
<script lang="ts">
	type NavLink = { label: string; href: string };

	type Props = {
		logo: string;
		logoAlt?: string;
		tagline?: string;
		links?: NavLink[];
		donateHref?: string;
		phone?: string;
		email?: string;
		facebookUrl?: string;
		serviceName?: string;
		serviceTimes?: { label: string; time: string }[];
		street?: string;
		cityLine?: string;
		legalName?: string;
		ein?: string;
	};

	let {
		logo,
		logoAlt = '1000 Hills Ministry International',
		tagline = 'Feeding body and soul in the fight against poverty, homelessness and addiction in Houston, Texas.',
		links = [
			{ label: 'About', href: '/about' },
			{ label: 'Services', href: '/services' },
			{ label: 'Contact', href: '/contact' }
		],
		donateHref = '/donate',
		phone = '281-787-8275',
		email,
		facebookUrl = 'https://www.facebook.com/1000Hills/',
		serviceName = 'Church in the Field',
		serviceTimes = [
			{ label: 'Mon – Sat', time: '7:30 PM' },
			{ label: 'Sunday', time: '3:00 PM' }
		],
		street = '2800 St. Charles St.',
		cityLine = 'Houston, TX 77004',
		legalName = '1000 Hills Ministry International Inc.',
		ein = '51-0666528'
	}: Props = $props();

	const year = new Date().getFullYear();
	const telHref = $derived(`tel:+1${phone.replace(/\D/g, '')}`);
	const directionsUrl = $derived(
		`https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(`${street}, ${cityLine}`)}`
	);
</script>

<footer class="footer">
	<div class="footer__inner">
		<div class="footer__brand">
			<a class="footer__logo" href="/" aria-label="1000 Hills Ministry home">
				<img src={logo} alt={logoAlt} />
			</a>
			{#if tagline}
				<p class="footer__tagline">{tagline}</p>
			{/if}
			<a class="footer__donate" href={donateHref}>Donate</a>
		</div>

		<nav class="footer__col" aria-label="Footer">
			<h2 class="footer__heading">Explore</h2>
			<ul>
				{#each links as link (link.href)}
					<li><a href={link.href}>{link.label}</a></li>
				{/each}
			</ul>
		</nav>

		<div class="footer__col">
			<h2 class="footer__heading">{serviceName}</h2>
			<ul class="footer__times">
				{#each serviceTimes as s (s.label)}
					<li><span>{s.label}</span> <strong>{s.time}</strong></li>
				{/each}
			</ul>
			<address>
				<a href={directionsUrl} target="_blank" rel="noopener noreferrer">
					{street}<br />{cityLine}
					<span class="sr-only">(opens directions in Google Maps)</span>
				</a>
			</address>
		</div>

		<div class="footer__col">
			<h2 class="footer__heading">Contact</h2>
			<ul>
				{#if phone}
					<li><a href={telHref}>{phone}</a></li>
				{/if}
				{#if email}
					<li><a href="mailto:{email}">{email}</a></li>
				{/if}
				{#if facebookUrl}
					<li>
						<a class="footer__social" href={facebookUrl} target="_blank" rel="noopener noreferrer">
							<svg viewBox="0 0 24 24" aria-hidden="true">
								<path
									d="M13.5 21v-7.5h2.6l.4-3h-3V8.6c0-.9.25-1.5 1.5-1.5h1.6V4.4a21 21 0 0 0-2.35-.12c-2.33 0-3.9 1.42-3.9 4.03v2.2H7.75v3h2.6V21h3.15Z"
								/>
							</svg>
							Facebook
							<span class="sr-only">(opens in a new tab)</span>
						</a>
					</li>
				{/if}
			</ul>
		</div>
	</div>

	<div class="footer__bottom">
		<div class="footer__bottom-inner">
			<p>© {year} {legalName} All rights reserved.</p>
			{#if ein}
				<p>A 501(c)(3) nonprofit · EIN {ein} · Donations are tax-deductible.</p>
			{/if}
		</div>
	</div>
</footer>

<style>
	.footer {
		--ft-navy: #0e1e3c;
		--ft-blue: #1f3f75;
		--ft-accent: #c85f1c;
		--ft-accent-hover: #a94d14;
		--ft-cream: #f6f3ee;
		--ft-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--ft-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		background: var(--ft-cream);
		color: var(--ft-navy);
		font-family: var(--ft-body-font);
		border-top: 4px solid var(--ft-accent);
	}

	.footer,
	.footer *,
	.footer *::before,
	.footer *::after {
		box-sizing: border-box;
	}

	.footer__inner {
		display: grid;
		gap: 2.25rem;
		max-width: 1200px;
		margin: 0 auto;
		padding: 3rem 1.25rem 2.5rem;
	}

	/* ---------- Brand column ---------- */
	.footer__logo {
		display: inline-block;
	}

	.footer__logo img {
		display: block;
		height: 72px;
		width: auto;
	}

	.footer__tagline {
		max-width: 22rem;
		margin: 1rem 0 0;
		font-size: 0.95rem;
		line-height: 1.6;
		color: rgb(14 30 60 / 0.75);
	}

	.footer__donate {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		min-height: 44px;
		margin-top: 1.25rem;
		padding: 0.6rem 1.5rem;
		border-radius: 4px;
		background: var(--ft-accent);
		color: #fff;
		font-family: var(--ft-display-font);
		font-size: 1.05rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		transition: background-color 0.2s ease;
	}

	/* ---------- Link columns ---------- */
	.footer__heading {
		margin: 0 0 0.9rem;
		font-family: var(--ft-display-font);
		font-size: 1.1rem;
		font-weight: 700;
		letter-spacing: 0.06em;
		text-transform: uppercase;
	}

	.footer ul {
		margin: 0;
		padding: 0;
		list-style: none;
	}

	.footer__col li + li {
		margin-top: 0.55rem;
	}

	.footer__col a {
		color: var(--ft-navy);
		text-decoration: none;
	}

	.footer__times li {
		display: flex;
		justify-content: space-between;
		gap: 1rem;
		max-width: 14rem;
	}

	.footer__times span {
		color: rgb(14 30 60 / 0.75);
	}

	address {
		margin-top: 0.9rem;
		font-style: normal;
		line-height: 1.5;
	}

	.footer__social {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
	}

	.footer__social svg {
		width: 1.25rem;
		height: 1.25rem;
		fill: var(--ft-blue);
	}

	/* ---------- Bottom bar ---------- */
	.footer__bottom {
		border-top: 1px solid rgb(14 30 60 / 0.12);
	}

	.footer__bottom-inner {
		display: flex;
		flex-direction: column;
		gap: 0.35rem;
		max-width: 1200px;
		margin: 0 auto;
		padding: 1.25rem 1.25rem 1.5rem;
		font-size: 0.85rem;
		line-height: 1.5;
		color: rgb(14 30 60 / 0.65);
	}

	.footer__bottom p {
		margin: 0;
	}

	/* ---------- Focus & hover ---------- */
	.footer a:focus-visible {
		outline: 3px solid var(--ft-accent);
		outline-offset: 3px;
		border-radius: 2px;
	}

	@media (hover: hover) {
		.footer__col a:hover {
			color: var(--ft-accent);
			text-decoration: underline;
			text-underline-offset: 3px;
		}
		.footer__donate:hover {
			background: var(--ft-accent-hover);
		}
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

	/* ---------- Tablet: brand on top, three columns below ---------- */
	@media (min-width: 640px) {
		.footer__inner {
			grid-template-columns: repeat(3, 1fr);
		}

		.footer__brand {
			grid-column: 1 / -1;
		}
	}

	/* ---------- Desktop: four columns ---------- */
	@media (min-width: 960px) {
		.footer__inner {
			grid-template-columns: 1.6fr 1fr 1.2fr 1fr;
			gap: 3rem;
			padding: 4rem 2rem 3rem;
		}

		.footer__brand {
			grid-column: auto;
		}

		.footer__bottom-inner {
			flex-direction: row;
			justify-content: space-between;
			padding: 1.25rem 2rem 1.5rem;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.footer__donate {
			transition: none;
		}
	}
</style>