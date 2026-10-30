<script lang="ts">
	import Heading from '#lib/components/Heading.svelte';
	import Text from '#lib/components/Text.svelte';
	import Button from '#lib/components/Button.svelte';
</script>

<svelte:head>
	<title>Church in the Field · 1000 Hills Ministry</title>
	<meta
		name="description"
		content="Every day, a church from around Houston gathers on a small patch of grass to share a meal, worship together, and remind our neighbors that God loves them."
	/>
</svelte:head>

<section class="hero" aria-labelledby="hero-title">
	<div class="hero-inner">
		<div class="hero-copy">
			<Text variant="eyebrow">Church in the Field · Houston, Texas</Text>

			<Heading level={1} id="hero-title">No one in Houston should face despair alone.</Heading>

			<Text size="l">
				Every day, a church from around Houston gathers on a small patch of grass to share a meal,
				worship together, and remind our neighbors that God loves them, and so do we.
			</Text>

			<div class="hero-actions">
				<Button href="/give" block="mobile">Give today</Button>
				<Button href="/volunteer" variant="secondary" block="mobile">Volunteer</Button>
			</div>
		</div>

		<figure class="hero-photo">
			<!--
				Swap this placeholder for the real photo, e.g.
				<img src="/images/church-in-the-field.jpg" alt="[Who is in the photo]" width="1200" height="900" />
			-->
			<div class="photo-placeholder">
				[Photo: phone selfie of the ministry leader with two volunteers and a friend from the field,
				smiling, in daylight]
			</div>
			<Text as="figcaption" variant="caption">[Names], at Church in the Field</Text>
		</figure>
	</div>
</section>

<svg class="hills" viewBox="0 0 1200 110" preserveAspectRatio="none" aria-hidden="true">
	<path class="hill-far" d="M0 56 C200 20 380 50 560 32 C760 12 960 50 1200 22 L1200 110 L0 110 Z" />
	<path class="hill-middle" d="M0 84 C220 54 420 80 640 64 C860 48 1020 76 1200 62 L1200 110 L0 110 Z" />
	<path class="hill-ground" d="M0 102 C260 84 500 100 760 90 C960 82 1100 96 1200 92 L1200 110 L0 110 Z" />
</svg>

<style>
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

	.hero-actions {
		display: flex;
		flex-wrap: wrap;
		gap: 0.875rem;
		padding-top: 0.375rem;
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

	/* Layered hills between the hero and the next (white) section */
	.hills {
		display: block;
		width: 100%;
		height: 110px;
		background: var(--color-background);
	}
	.hill-far { fill: var(--color-hill-far); }
	.hill-middle { fill: var(--color-hill-middle); }
	.hill-ground { fill: var(--color-surface); }

	@media (max-width: 900px) {
		.hero-inner {
			padding: 2.5rem var(--space-m) var(--space-xl);
		}
		.hills {
			height: 64px;
		}
	}
</style>
