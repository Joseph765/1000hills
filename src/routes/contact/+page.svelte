<script lang="ts">
	import Heading from '#lib/components/Heading.svelte';
	import Text from '#lib/components/Text.svelte';
	import Button from '#lib/components/Button.svelte';
	import HillsDivider from '#lib/components/HillsDivider.svelte';
	import ServiceTimes from '#lib/components/ServiceTimes.svelte';

	const director = {
		name: 'Lee Whitlock',
		role: 'Director',
		phone: '(281) 787-8275',
		email: 'lee.whitlock1993@gmail.com'
	};

	const mailing = {
		name: '1000 Hills Ministry',
		lines: ['PO Box 591262', 'Houston, TX 77259']
	};

	const board = [
		{ name: 'Pat Lavergne', role: 'Secretary/Treasurer' },
		{ name: 'Lisa Degner' },
		{ name: 'Richard Degner' },
		{ name: 'Dr. Joseph McCollum' },
		{ name: 'Joel Mueller' }
	];

	const telHref = `tel:+1${director.phone.replace(/\D/g, '')}`;
</script>

<svelte:head>
	<title>Contact | 1000 Hills Ministry</title>
	<meta
		name="description"
		content="Contact 1000 Hills Ministry about giving, volunteering, or bringing your church, group or family to serve. Call (281) 787-8275."
	/>
</svelte:head>

<section class="hero" aria-labelledby="hero-title">
	<div class="hero-inner">
		<Text variant="eyebrow">Contact us</Text>
		<Heading level={1} id="hero-title">Get in touch</Heading>
		<Text size="l">
			Questions about giving, volunteering, or how you and your group or family can get involved?
			We'd love to hear from you.
		</Text>
	</div>
</section>

<HillsDivider groundColor="var(--color-surface)" />

<section class="director" aria-labelledby="director-title">
	<div class="director-inner">
		<div class="director-copy">
			<Text variant="eyebrow">{director.role}</Text>
			<Heading level={2} id="director-title">{director.name}</Heading>
			<ul class="director-details">
				<li><Text as="span" size="l" tone="link"><a href={telHref}>{director.phone}</a></Text></li>
				<li>
					<Text as="span" size="l" tone="link"
						><a href="mailto:{director.email}">{director.email}</a></Text
					>
				</li>
			</ul>
			<div class="director-actions">
				<Button href={telHref} block="mobile">Call</Button>
				<Button href="mailto:{director.email}" variant="secondary" block="mobile">Email</Button>
			</div>
		</div>

		<figure class="director-photo">
			<!--
				Swap this placeholder for a real photo, e.g.
				<img src="/images/....jpg" alt="Lee Whitlock, director of 1000 Hills Ministry" width="900" height="1125" />
			-->
			<div class="photo-placeholder">[Photo: Lee Whitlock, the director]</div>
		</figure>
	</div>
</section>

<ServiceTimes id="our-church" eyebrow="Our church" showMap />

<section class="more" aria-label="Mailing address and board of directors">
	<div class="more-inner">
		<Text variant="eyebrow">Mailing &amp; leadership</Text>
		<div class="more-cards">
			<div class="card">
				<Text variant="eyebrow">Mailing address</Text>
				<Heading level={3} size="l">{mailing.name}</Heading>
				<address class="card-address">
					{#each mailing.lines as line (line)}
						{line}<br />
					{/each}
				</address>
			</div>

			<div class="card">
				<Text variant="eyebrow">Leadership</Text>
				<Heading level={3} size="l">Board of Directors</Heading>
				<ul class="board">
					{#each board as member (member.name)}
						<li>
							<Text as="span" weight="strong">{member.name}</Text>
							{#if member.role}<Text as="span" size="s" tone="weak">{member.role}</Text>{/if}
						</li>
					{/each}
				</ul>
			</div>
		</div>
	</div>
</section>

<style>
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

	/* ---------- Director (white, photo + contact) ---------- */
	.director {
		background: var(--color-surface);
	}

	.director-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxl) var(--size-gutter);
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: var(--space-xxl) 3.5rem;
	}

	.director-copy {
		flex: 1 1 400px;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	.director-details {
		margin: 0;
		padding: 0;
		list-style: none;
		display: flex;
		flex-direction: column;
		gap: var(--space-xs);
	}

	.director-details a:hover {
		color: var(--color-text-link-hover);
	}

	.director-actions {
		display: flex;
		flex-wrap: wrap;
		gap: var(--space-xs);
		padding-top: var(--space-xxs);
	}

	.director-photo {
		flex: 1 1 280px;
		min-width: 0;
		margin: 0;
	}

	.photo-placeholder {
		aspect-ratio: 4 / 5;
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

	/* ---------- Mailing & leadership (white) ---------- */
	.more {
		background: var(--color-surface);
	}

	.more-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		padding: var(--space-xxl) var(--size-gutter) var(--space-xxxl);
	}

	.more-cards {
		margin-top: var(--space-m);
		display: flex;
		flex-wrap: wrap;
		align-items: flex-start;
		gap: var(--space-m);
	}

	.card {
		flex: 1 1 280px;
		min-width: 0;
		padding: var(--space-l);
		display: flex;
		flex-direction: column;
		gap: var(--space-xs);
		background: var(--color-surface-tint);
		border-radius: var(--border-radius);
	}

	.card-address {
		margin-top: var(--space-xs);
		font-style: normal;
		font-family: var(--font-family);
		font-size: var(--font-size-m);
		line-height: var(--line-height);
		color: var(--color-text-weak);
	}

	.board {
		margin: var(--space-xs) 0 0;
		padding: 0;
		list-style: none;
	}

	.board li {
		display: flex;
		flex-wrap: wrap;
		align-items: baseline;
		justify-content: space-between;
		gap: 0 var(--space-m);
		padding: var(--space-s) 0;
		border-bottom: var(--border-width) solid var(--color-border);
	}

	.board li:last-child {
		border-bottom: 0;
	}

	@media (max-width: 900px) {
		.hero-inner,
		.director-inner,
		.more-inner {
			padding-inline: var(--space-m);
		}
		.hero-inner {
			padding-block: 2.5rem var(--space-xl);
		}
	}
</style>
