<script lang="ts">
	import Heading from '#lib/components/Heading.svelte';
	import Text from '#lib/components/Text.svelte';
	import Button from '#lib/components/Button.svelte';
	import HillsDivider from '#lib/components/HillsDivider.svelte';

	const happens = [
		{
			title: 'A different church each day',
			text: 'Churches across Houston take turns hosting, so the field is never empty.'
		},
		{
			title: 'A meal to share',
			text: "Often it's pizza. It's always served with a smile and a conversation."
		},
		{
			title: 'Worship and the Word',
			text: 'Songs, prayer, and a message of hope for anyone who needs it.'
		}
	];
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
				<Button href="https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU" block="mobile" target="_blank">Give today</Button>
				<Button href="/contact" variant="secondary" block="mobile">Volunteer</Button>
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

<HillsDivider groundColor="var(--color-surface)" />

<section class="happens" aria-labelledby="happens-title">
	<div class="happens-inner">
		<div class="happens-header">
			<Heading level={2} id="happens-title">What happens in the field</Heading>
			<Text size="l">
				Nothing fancy. Just neighbors, a meal, and the good news, every single day.
			</Text>
		</div>

		<ul class="cards">
			{#each happens as item (item.title)}
				<li class="card">
					<Heading level={3} size="l">{item.title}</Heading>
					<Text>{item.text}</Text>
				</li>
			{/each}
		</ul>
	</div>
</section>

<section class="donate" aria-labelledby="donate-title">
	<div class="donate-inner">
		<div class="donate-copy">
			<Heading level={2} id="donate-title" tone="on-accent">
				$100 feeds everyone in the field for one night.
			</Heading>
			<Text size="l" tone="on-accent-weak">
				Your gift buys the food and [what else a gift covers]. Every dollar stays here in
				Houston.
			</Text>
		</div>

		<Button
			href="https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU"
			variant="inverse"
			block="mobile"
			target="_blank"
			rel="noopener noreferrer"
		>
			Give today<span class="visually-hidden"> (opens in a new tab)</span>
		</Button>
	</div>
</section>

<section class="meal" aria-label="A word about serving">
	<div class="meal-inner">
		<figure class="meal-photo">
			<!--
				Swap this placeholder for the real photo, e.g.
				<img src="/images/....jpg" alt="[Who is in the photo]" width="1200" height="900" />
			-->
			<div class="photo-placeholder">
				[Photo: a volunteer serving a plate of food to someone at Church in the Field]
			</div>
		</figure>

		<figure class="meal-copy">
			<blockquote>
				<Text as="p" variant="caption" size="l" tone="default" measure={false}>
					"For I was hungry and you gave me food, I was thirsty and you gave me drink, I was a
					stranger and you welcomed me."
				</Text>
			</blockquote>
			<Text as="figcaption" variant="eyebrow">Matthew 25:35 · ESV</Text>
		</figure>
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

	/* ---------- What happens in the field (white) ---------- */
	.happens {
		background: var(--color-surface);
	}

	.happens-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter);
		display: flex;
		flex-direction: column;
		gap: var(--space-xl);
	}

	.happens-header {
		max-width: 40rem;
		display: flex;
		flex-direction: column;
		gap: var(--space-s);
	}

	.cards {
		margin: 0;
		padding: 0;
		list-style: none;
		display: flex;
		flex-wrap: wrap;
		gap: var(--space-m);
	}

	.card {
		flex: 1 1 260px;
		min-width: 0;
		padding: var(--space-l);
		display: flex;
		flex-direction: column;
		gap: var(--space-xs);
		background: var(--color-surface-tint);
		border-radius: var(--border-radius);
	}

	/* ---------- Donate (navy) ---------- */
	.donate {
		background: var(--color-surface-accent);
	}

	.donate-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter);
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		justify-content: space-between;
		gap: var(--space-xl);
	}

	.donate-copy {
		flex: 1 1 520px;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	/* ---------- A word about serving (mist) ---------- */
	.meal {
		background: var(--color-background);
	}

	.meal-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxxl) var(--size-gutter);
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: var(--space-xxl) 3.5rem;
	}

	.meal-photo {
		flex: 1 1 420px;
		min-width: 0;
		margin: 0;
	}

	.meal-copy {
		flex: 1 1 440px;
		min-width: 0;
		margin: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-xs);
	}

	.meal-copy blockquote {
		margin: 0;
	}

	@media (max-width: 900px) {
		.hero-inner {
			padding: 2.5rem var(--space-m) var(--space-xl);
		}
		.happens-inner,
		.donate-inner,
		.meal-inner {
			padding-inline: var(--space-m);
		}
	}
</style>
