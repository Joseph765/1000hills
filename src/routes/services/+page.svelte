<script lang="ts">
	import PageBanner from '#lib/components/PageBanner.svelte';
	import ServiceTimes from '#lib/components/ServiceTimes.svelte';
	import VideoEmbed from '#lib/components/VideoEmbed.svelte';

	const expect = [
		{
			title: 'Praise & worship',
			text: 'Each service includes praise and worship, led by the local churches that come to serve alongside us.'
		},
		{
			title: "God's Word",
			text: 'The Gospel of the Lord Jesus Christ is preached at every service, every day of the year.'
		},
		{
			title: 'Prayer',
			text: 'Prayer is part of every service. We bring our needs, and the needs of our neighbors, to God together.'
		},
		{
			title: 'A meal',
			text: 'A meal is served at every service, and we do our best to meet immediate, pressing needs.'
		}
	];
</script>

<svelte:head>
	<title>Services | 1000 Hills Ministry International</title>
	<meta
		name="description"
		content="Church in the Field meets Monday through Saturday at 7:30 PM and Sunday at 3 PM at 2800 St. Charles St., Houston. Worship, the Word, prayer and a meal. Everyone is welcome."
	/>
</svelte:head>

<PageBanner
	image="/images/tent.png"
	imageAlt="People gathered for an outdoor church service"
	eyebrow="Services"
	title="Join us in the field"
	intro="Church in the Field meets outdoors seven days a week. Come as you are."
/>

<ServiceTimes eyebrow="Service times" />

<!-- ============ What to expect ============ -->
<section class="section" aria-labelledby="expect-heading">
	<div class="container">
		<p class="eyebrow">What to expect</p>
		<h2 id="expect-heading" class="heading">Everything you'd expect from church, outdoors</h2>
		<p class="intro">
			You don't need to bring anything. Chairs, tables, speakers and lights are set up before every
			service by the men of our Disciple House.
		</p>

		<ul class="expect">
			{#each expect as item, i (item.title)}
				<li class="expect__item">
					<span class="expect__num" aria-hidden="true">{String(i + 1).padStart(2, '0')}</span>
					<h3 class="expect__title">{item.title}</h3>
					<p>{item.text}</p>
				</li>
			{/each}
		</ul>
	</div>
</section>

<!-- ============ Serve with us ============ -->
<section class="cta" aria-labelledby="cta-heading">
	<div class="cta__inner">
		<div class="cta__copy">
			<p class="cta__eyebrow">Serve with us</p>
			<h2 id="cta-heading" class="cta__heading">Bring your church or group</h2>
			<p class="cta__text">
				Local churches and ministries regularly lead our outdoor services. It's a real mission field
				without leaving the country.
			</p>
			<a class="cta__button" href="/contact">Contact us</a>
		</div>

		<figure class="cta__video">
			<VideoEmbed provider="youtube" id="7Zq4LNAjkrs" title="Church Under the Bridge, by Cru Houston" />
			<figcaption>
				A Cru Houston group serving at Church Under the Bridge, January 2015.
				<span class="cta__credit">
					Video by
					<a href="https://www.youtube.com/@cruhoustonvideos" target="_blank" rel="noopener noreferrer"
						>Cru Houston<span class="sr-only"> (opens in a new tab)</span></a
					>
				</span>
			</figcaption>
		</figure>
	</div>
</section>

<style>
	.section,
	.cta {
		--navy: #0e1e3c;
		--accent: #c85f1c;
		--accent-hover: #a94d14;
		--cream: #f6f3ee;
		--display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		font-family: var(--body-font);
	}

	.section,
	.section *,
	.cta,
	.cta * {
		box-sizing: border-box;
	}

	/* ---------- What to expect ---------- */
	.section {
		padding: 3.5rem 1.25rem;
		background: #fff;
		color: var(--navy);
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
	}

	.eyebrow {
		margin: 0 0 0.75rem;
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: var(--accent);
	}

	.heading {
		max-width: 20ch;
		margin: 0;
		font-family: var(--display-font);
		font-size: clamp(2rem, 8vw, 2.5rem);
		font-weight: 700;
		line-height: 1.05;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.intro {
		max-width: 44rem;
		margin: 1rem 0 0;
		font-size: 1.05rem;
		line-height: 1.6;
		color: rgb(14 30 60 / 0.8);
	}

	.expect {
		display: grid;
		gap: 1rem;
		margin: 2rem 0 0;
		padding: 0;
		list-style: none;
	}

	.expect__item {
		padding: 1.5rem;
		border-top: 4px solid var(--accent);
		border-radius: 8px;
		background: var(--cream);
	}

	.expect__num {
		display: block;
		font-family: var(--display-font);
		font-size: 2rem;
		font-weight: 700;
		line-height: 1;
		color: var(--accent);
	}

	.expect__title {
		margin: 0.6rem 0 0.4rem;
		font-family: var(--display-font);
		font-size: 1.4rem;
		font-weight: 700;
		line-height: 1.15;
		text-transform: uppercase;
	}

	.expect__item p {
		margin: 0;
		font-size: 1rem;
		line-height: 1.6;
		color: rgb(14 30 60 / 0.8);
	}

	/* ---------- Call to action ---------- */
	.cta {
		padding: 3.5rem 1.25rem;
		background: var(--navy);
		color: #fff;
	}

	.cta__inner {
		display: grid;
		gap: 2.25rem;
		max-width: 1200px;
		margin: 0 auto;
	}

	.cta__eyebrow {
		margin: 0 0 0.75rem;
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: #f3b27f;
	}

	.cta__heading {
		margin: 0;
		font-family: var(--display-font);
		font-size: clamp(2rem, 8vw, 2.5rem);
		font-weight: 700;
		line-height: 1.05;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.cta__text {
		max-width: 34rem;
		margin: 1rem 0 0;
		font-size: 1.05rem;
		line-height: 1.6;
		color: rgb(255 255 255 / 0.8);
	}

	.cta__button {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		min-height: 48px;
		margin-top: 1.75rem;
		padding: 0.8rem 1.75rem;
		border-radius: 4px;
		background: var(--accent);
		color: #fff;
		font-family: var(--display-font);
		font-size: 1.1rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		transition: background-color 0.2s ease;
	}

	.cta__button:focus-visible {
		outline: 3px solid #fff;
		outline-offset: 3px;
	}

	.cta__video {
		margin: 0;
	}

	.cta__video figcaption {
		margin-top: 0.85rem;
		font-size: 0.875rem;
		line-height: 1.5;
		color: rgb(255 255 255 / 0.6);
	}

	.cta__credit {
		white-space: nowrap;
	}

	.cta__credit a {
		color: #f3b27f;
		font-weight: 600;
		text-decoration: underline;
		text-decoration-color: rgb(243 178 127 / 0.4);
		text-underline-offset: 3px;
	}

	.cta__credit a:focus-visible {
		outline: 3px solid #f3b27f;
		outline-offset: 2px;
		border-radius: 2px;
	}

	@media (hover: hover) {
		.cta__button:hover {
			background: var(--accent-hover);
		}
		.cta__credit a:hover {
			text-decoration-color: currentColor;
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

	/* ---------- Tablet ---------- */
	@media (min-width: 640px) {
		.expect {
			grid-template-columns: repeat(2, 1fr);
		}

	}

	/* ---------- Desktop ---------- */
	@media (min-width: 900px) {
		.section {
			padding: 5.5rem 2rem;
		}

		.eyebrow {
			font-size: 0.875rem;
		}

		.heading {
			font-size: clamp(2.75rem, 4.5vw, 3.75rem);
		}

		.intro {
			font-size: 1.15rem;
		}

		.expect {
			grid-template-columns: repeat(4, 1fr);
			gap: 1.5rem;
			margin-top: 2.75rem;
		}

		.cta {
			padding: 5.5rem 2rem;
		}

		.cta__inner {
			grid-template-columns: 1fr 1.15fr;
			align-items: center;
			gap: 4rem;
		}

		.cta__heading {
			font-size: clamp(2.5rem, 4vw, 3.25rem);
		}

		.cta__eyebrow {
			font-size: 0.875rem;
		}

		.cta__text {
			font-size: 1.15rem;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.cta__button {
			transition: none;
		}
	}
</style>