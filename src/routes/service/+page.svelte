<script lang="ts">
	import Heading from '#lib/components/Heading.svelte';
	import Text from '#lib/components/Text.svelte';
	import Button from '#lib/components/Button.svelte';
	import HillsDivider from '#lib/components/HillsDivider.svelte';
	import ServiceTimes from '#lib/components/ServiceTimes.svelte';

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
	<title>Service | 1000 Hills Ministry</title>
	<meta
		name="description"
		content="Church in the Field meets Monday through Saturday at 7:30 PM and Sunday at 3 PM at 2800 St. Charles St., Houston. Worship, the Word, prayer and a meal. Everyone is welcome."
	/>
</svelte:head>

<section class="hero" aria-labelledby="hero-title">
	<div class="hero-inner">
		<Text variant="eyebrow">Service</Text>
		<Heading level={1} id="hero-title">Join us in the field</Heading>
		<Text size="l">Church in the Field meets outdoors seven days a week. Come as you are.</Text>
	</div>
</section>

<HillsDivider groundColor="var(--color-background)" />

<ServiceTimes eyebrow="Service times" showMap />

<section class="expect" aria-labelledby="expect-title">
	<div class="expect-inner">
		<Text variant="eyebrow">What to expect</Text>
		<Heading level={2} id="expect-title">Everything you'd expect from church, outdoors</Heading>
		<Text size="l">
			You don't need to bring anything. Chairs, tables, speakers and lights are set up before
			every service by the men of our Disciple House.
		</Text>

		<ul class="cards">
			{#each expect as item (item.title)}
				<li class="card">
					<Heading level={3} size="l">{item.title}</Heading>
					<Text>{item.text}</Text>
				</li>
			{/each}
		</ul>
	</div>
</section>

<section class="cta" aria-labelledby="cta-title">
	<div class="cta-inner">
		<Heading level={2} id="cta-title">Come as you are</Heading>
		<Text size="l">
			God loves the people of Houston, and so do we. Give, volunteer, or bring your church for a
			night in the field.
		</Text>
		<div class="cta-actions">
			<Button
				href="https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU"
				block="mobile"
				target="_blank"
				rel="noopener noreferrer"
			>
				Give today<span class="visually-hidden"> (opens in a new tab)</span>
			</Button>
			<Button href="/contact" variant="secondary" block="mobile">Volunteer</Button>
		</div>
	</div>
</section>

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

	/* ---------- Hero ---------- */
	.hero {
		background: var(--color-background);
	}

	.hero-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter) var(--space-xxl);
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	/* ---------- What to expect (white) ---------- */
	.expect {
		background: var(--color-surface);
	}

	.expect-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxl) var(--size-gutter) var(--space-xxxl);
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	.cards {
		margin: var(--space-m) 0 0;
		padding: 0;
		list-style: none;
		display: flex;
		flex-wrap: wrap;
		gap: var(--space-m);
	}

	.card {
		flex: 1 1 220px;
		min-width: 0;
		padding: var(--space-l);
		display: flex;
		flex-direction: column;
		gap: var(--space-xs);
		background: var(--color-surface-tint);
		border-radius: var(--border-radius);
	}

	/* ---------- Closing CTA (mist) ---------- */
	.cta {
		background: var(--color-background);
	}

	.cta-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter);
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	.cta-actions {
		display: flex;
		flex-wrap: wrap;
		gap: 0.875rem;
		padding-top: 0.375rem;
	}

	@media (max-width: 900px) {
		.hero-inner,
		.expect-inner,
		.cta-inner {
			padding-inline: var(--space-m);
		}
		.hero-inner {
			padding-block: 2.5rem var(--space-xl);
		}
	}
</style>
