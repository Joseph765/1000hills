<script lang="ts">
	import Heading from '#lib/components/Heading.svelte';
	import Text from '#lib/components/Text.svelte';
	import Button from '#lib/components/Button.svelte';
	import HillsDivider from '#lib/components/HillsDivider.svelte';
	import VideoEmbed from '#lib/components/VideoEmbed.svelte';
</script>

<svelte:head>
	<title>About | 1000 Hills Ministry International</title>
	<meta
		name="description"
		content="1000 Hills is a local family of churches who show up every day, so no one in Houston faces despair alone. Our mission, what we believe, and the Disciple House."
	/>
</svelte:head>

<section class="hero" aria-labelledby="hero-title">
	<div class="hero-inner">
		<div class="hero-copy">
			<Text variant="eyebrow">About us</Text>

			<Heading level={1} id="hero-title">
				To serve the poorest of the poor, in any community where God leads us to serve.
			</Heading>

			<Text size="l">
				Every day, we preach the Gospel of Jesus Christ and serve a meal, outdoors, to Houston's
				homeless and families in extreme poverty. We're not primarily a food ministry — we're a
				Jesus ministry.
			</Text>
		</div>

		<figure class="hero-photo">
			<!--
				Swap this placeholder for a real photo, e.g.
				<img src="/images/....jpg" alt="[Who is in the photo]" width="1200" height="900" />
			-->
			<div class="photo-placeholder">
				[Photo: the disciples or a visiting church serving a meal at Church in the Field, candid,
				daylight]
			</div>
			<Text as="figcaption" variant="caption">[Names], at Church in the Field</Text>
		</figure>
	</div>
</section>

<HillsDivider groundColor="var(--color-surface)" />

<section class="believe" aria-labelledby="believe-title">
	<div class="believe-inner">
		<Text variant="eyebrow">What we believe</Text>
		<Heading level={2} id="believe-title">Saved by grace, through faith in Jesus</Heading>
		<Text size="l">
			We're not saved by being good or by our own works. We're saved by His grace, through faith
			in what Jesus already did: His death, burial and resurrection. Faith plus nothing.
		</Text>

		<figure class="verse">
			<blockquote>
				<Text as="p" variant="caption" size="l" tone="default" measure={false}>
					"For I delivered to you as of first importance what I also received: that Christ died
					for our sins in accordance with the Scriptures, that he was buried, that he was raised
					on the third day in accordance with the Scriptures."
				</Text>
			</blockquote>
			<Text as="figcaption" variant="eyebrow">1 Corinthians 15:3–4 · ESV</Text>
		</figure>
	</div>
</section>

<section class="disciple-house" aria-labelledby="disciple-house-title">
	<div class="disciple-house-inner">
		<div class="disciple-house-card">
			<Text variant="eyebrow">Behind every service</Text>
			<Heading level={2} id="disciple-house-title" size="xl">The Disciple House</Heading>
			<Text size="l">
				1000 Hills rents a home in Houston for a group of men we call disciples — the Disciple
				House. They run the ministry day to day, setting up and breaking down every service, and
				they're a steady, daily presence for neighbors nearby, especially single mothers and
				elderly neighbors who come for food, clothing and other needs.
			</Text>
		</div>
	</div>
</section>

<section class="documentary" aria-labelledby="documentary-title">
	<div class="documentary-inner">
		<div class="documentary-copy">
			<Text variant="eyebrow">See us in action</Text>
			<Heading level={2} id="documentary-title">The ministry, up close</Heading>
			<Text size="l">
				A short documentary about 1000 Hills Ministry and the Houston neighbors we serve.
			</Text>
		</div>

		<figure class="player">
			<VideoEmbed
				provider="vimeo"
				id="16186350"
				title="1000 Hills Ministry International documentary"
			/>
			<figcaption>
				<Text as="span" variant="caption">Documentary · 10 min</Text>
				<Text as="span" size="s">
					Thank you to
					<a href="https://www.homerfilms.com" target="_blank" rel="noopener noreferrer"
						>Homer Films<span class="visually-hidden"> (opens in a new tab)</span></a
					>
					for producing it!
				</Text>
			</figcaption>
		</figure>
	</div>
</section>

<section class="cta" aria-labelledby="cta-title">
	<div class="cta-inner">
		<Heading level={2} id="cta-title">Be part of it</Heading>
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

	/* ---------- Hero (same pattern as the homepage hero) ---------- */
	.hero {
		background: var(--color-background);
	}

	.hero-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter) var(--space-xxl);
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: var(--space-xxl) 3.5rem;
	}

	.hero-copy {
		flex: 1 1 440px;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: 1.375rem;
	}

	.hero-photo {
		flex: 1 1 420px;
		min-width: 0;
		margin: 0;
		display: flex;
		flex-direction: column;
		gap: 0.625rem;
	}

	.hero-photo :global(img) {
		display: block;
		width: 100%;
		height: auto;
		aspect-ratio: 4 / 3;
		object-fit: cover;
		border-radius: var(--border-radius);
	}

	.photo-placeholder {
		aspect-ratio: 4 / 3;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: var(--space-xl);
		box-sizing: border-box;
		text-align: center;
		font-size: var(--font-size-m);
		line-height: 1.5;
		color: var(--color-text);
		background: var(--color-surface-placeholder);
		border: var(--border-width-strong) dashed var(--color-border-decorative);
		border-radius: var(--border-radius);
	}

	/* ---------- What we believe (white) ---------- */
	.believe {
		background: var(--color-surface);
	}

	.believe-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxl) var(--size-gutter);
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	.verse {
		margin: var(--space-l) 0 0;
		padding: var(--space-l) var(--space-xl);
		max-width: 44rem;
		background: var(--color-surface-tint);
		border-left: var(--border-width-strong) solid var(--color-border-strong);
		border-radius: 0 var(--border-radius) var(--border-radius) 0;
	}

	.verse blockquote {
		margin: 0 0 var(--space-xs);
	}

	/* ---------- Disciple House (mist, card on white) ---------- */
	.disciple-house {
		background: var(--color-background);
	}

	.disciple-house-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxl) var(--size-gutter);
	}

	.disciple-house-card {
		max-width: 44rem;
		padding: var(--space-xl);
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
		background: var(--color-surface);
		border-radius: var(--border-radius);
	}

	/* ---------- Documentary (light-blue feature band, centered) ---------- */
	.documentary {
		background: var(--color-surface-feature);
	}

	.documentary-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter);
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: var(--space-xl);
	}

	.documentary-copy {
		max-width: 40rem;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: var(--space-s);
		text-align: center;
	}

	.player {
		width: 100%;
		max-width: 48rem;
		margin: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-s);
	}

	.player figcaption {
		display: flex;
		flex-direction: column;
		gap: var(--space-xxs);
		text-align: center;
	}

	.player figcaption :global(a) {
		color: var(--color-text-link);
		font-weight: var(--font-weight-strong);
	}

	.player figcaption :global(a:hover) {
		color: var(--color-text-link-hover);
	}

	.player figcaption :global(a:focus-visible) {
		outline: 3px solid var(--color-text-link);
		outline-offset: 2px;
		border-radius: var(--border-radius-s);
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
		.believe-inner,
		.disciple-house-inner,
		.documentary-inner,
		.cta-inner {
			padding-inline: var(--space-m);
		}
		.hero-inner {
			padding-block: 2.5rem var(--space-xl);
		}
		.disciple-house-card {
			padding: var(--space-l);
		}
	}
</style>
