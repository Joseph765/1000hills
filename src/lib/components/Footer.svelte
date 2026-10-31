<!--
  Footer
  The site footer: logo + tagline, contact, and the service address,
  with a copyright line at the bottom. Used once in +layout.svelte,
  so it's on every page.

  <Footer />
-->
<script lang="ts">
	import Text from './Text.svelte';

	type Props = {
		logoSrc?: string;
		tagline?: string;
		phone?: string;
		email?: string;
		facebookUrl?: string;
		serviceName?: string;
		street?: string;
		cityLine?: string;
		legalName?: string;
		ein?: string;
	};

	let {
		logoSrc = '/images/1000hills-logo.png',
		tagline = 'Feeding body and soul in Houston, Texas.',
		phone = '(281) 787-8275',
		email,
		facebookUrl = 'https://www.facebook.com/1000Hills/',
		serviceName = 'Church in the Field',
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
	<div class="footer-inner">
		<div class="footer-brand">
			<a href="/" aria-label="1000 Hills Ministry home">
				<img src={logoSrc} alt="1000 Hills Ministry International" width="218" height="145" />
			</a>
			{#if tagline}
				<Text size="s" tone="weak">{tagline}</Text>
			{/if}
			<Text as="p" size="xs" tone="weak" class="footer-copyright">
				© {year} {legalName} All rights reserved.
			</Text>
			<!-- Commented out for now, until the nonprofit status/EIN is confirmed for the new site.
			{#if ein}
				<Text as="p" size="xs" tone="weak">
					A 501(c)(3) nonprofit · EIN {ein} · Donations are tax-deductible.
				</Text>
			{/if}
			-->
		</div>

		<div class="footer-col">
			<Text weight="active">Contact</Text>
			<ul>
				{#if phone}
					<li><a href={telHref}>{phone}</a></li>
				{/if}
				{#if email}
					<li><a href="mailto:{email}">{email}</a></li>
				{/if}
				{#if facebookUrl}
					<li>
						<a href={facebookUrl} target="_blank" rel="noopener noreferrer">
							Facebook<span class="visually-hidden"> (opens in a new tab)</span>
						</a>
					</li>
				{/if}
			</ul>
		</div>

		<div class="footer-col">
			<Text weight="active">{serviceName}</Text>
			<address>
				<a href={directionsUrl} target="_blank" rel="noopener noreferrer">
					{street}<br />{cityLine}
					<span class="visually-hidden"> (opens directions in Google Maps)</span>
				</a>
			</address>
		</div>
	</div>
</footer>

<style>
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

	.footer {
		background: var(--color-surface);
		border-top: var(--border-width) solid var(--color-border);
	}

	.footer-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxl) var(--size-gutter);
		display: flex;
		flex-wrap: wrap;
		gap: var(--space-xl);
	}

	.footer-brand {
		flex: 1 1 260px;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-s);
	}

	.footer-brand img {
		display: block;
		height: 52px;
		width: auto;
	}

	.footer-col {
		flex: 1 1 200px;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-xs);
	}

	.footer-col ul {
		margin: 0;
		padding: 0;
		list-style: none;
		display: flex;
		flex-direction: column;
		gap: var(--space-xxs);
	}

	address {
		font-style: normal;
	}

	.footer a {
		color: var(--color-text-link);
		font-size: var(--font-size-s);
		text-decoration: none;
	}
	.footer a:hover {
		color: var(--color-text-link-hover);
		text-decoration: underline;
		text-underline-offset: 3px;
	}
	.footer a:focus-visible {
		outline: 3px solid var(--color-text-link);
		outline-offset: 3px;
		border-radius: var(--border-radius-s);
	}

	@media (max-width: 900px) {
		.footer-inner {
			padding: var(--space-xxl) var(--space-m) var(--space-l) var(--space-m);
		}
	}
</style>
