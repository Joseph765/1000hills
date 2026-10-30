<!--
  WhatWeDo.svelte — "What we do" section for 1000 Hills Ministry
  Place in: src/lib/components/WhatWeDo.svelte and render it after <ServiceTimes /> on the homepage.
  Svelte 5 (runes). Each card shows a photo if you pass `image`, otherwise a simple icon.
-->
<script lang="ts">
	type Icon = 'cross' | 'meal' | 'heart';

	type Item = {
		title: string;
		text: string;
		icon?: Icon;
		image?: string;
		imageAlt?: string;
		href?: string;
		linkLabel?: string;
	};

	type Props = {
		id?: string;
		eyebrow?: string;
		heading?: string;
		intro?: string;
		items?: Item[];
	};

	let {
		id = 'what-we-do',
		eyebrow = 'What we do',
		heading = 'Food for the soul. Food for the body.',
		intro = 'We serve neighbors in Houston struggling with poverty, homelessness, addiction and despair, meeting them where they are with the Gospel and a meal.',
		items = [
			{
				icon: 'cross',
				title: 'Outdoor worship',
				text: 'Church in the Field meets seven days a week, bringing worship and the Word to people who may never walk into a church building.'
			},
			{
				icon: 'meal',
				title: 'Meals',
				text: 'Nutritious food for the body, served alongside spiritual food for the soul at every service.'
			},
			{
				icon: 'heart',
				title: 'Prayer & discipleship',
				text: 'We pray with people facing desperate situations and walk with them as they grow in faith: souls saved and saints edified.'
			}
		]
	}: Props = $props();
</script>

<section class="wwd" {id} aria-labelledby="{id}-heading">
	<div class="wwd__inner">
		<header class="wwd__header">
			{#if eyebrow}
				<p class="wwd__eyebrow">{eyebrow}</p>
			{/if}
			<h2 id="{id}-heading" class="wwd__heading">{heading}</h2>
			{#if intro}
				<p class="wwd__intro">{intro}</p>
			{/if}
		</header>

		<ul class="cards">
			{#each items as item (item.title)}
				<li class="card" class:has-image={item.image}>
					{#if item.image}
						<img class="card__img" src={item.image} alt={item.imageAlt ?? ''} loading="lazy" />
					{:else if item.icon}
						<span class="card__icon" aria-hidden="true">
							<svg viewBox="0 0 24 24">
								{#if item.icon === 'cross'}
									<path d="M12 3v18M6.5 8.5h11" />
								{:else if item.icon === 'meal'}
									<path d="M3 13.5h18a9 7.5 0 0 1-18 0Z" />
									<path
										d="M8 2.5c-1.3 1.5 1.3 2.75 0 4.25s1.3 2.5 0 3.75M12 2.5c-1.3 1.5 1.3 2.75 0 4.25s1.3 2.5 0 3.75M16 2.5c-1.3 1.5 1.3 2.75 0 4.25s1.3 2.5 0 3.75"
									/>
								{:else if item.icon === 'heart'}
									<path
										d="M12 20s-7.5-4.5-7.5-10.25A4.25 4.25 0 0 1 12 7.2a4.25 4.25 0 0 1 7.5 2.55C19.5 15.5 12 20 12 20Z"
									/>
								{/if}
							</svg>
						</span>
					{/if}

					<div class="card__body">
						<h3 class="card__title">{item.title}</h3>
						<p class="card__text">{item.text}</p>
						{#if item.href}
							<a class="card__link" href={item.href}>
								{item.linkLabel ?? 'Learn more'}<span aria-hidden="true"> →</span>
								<span class="sr-only"> about {item.title}</span>
							</a>
						{/if}
					</div>
				</li>
			{/each}
		</ul>
	</div>
</section>

<style>
	.wwd {
		--wwd-navy: #0e1e3c;
		--wwd-blue: #1f3f75;
		--wwd-accent: #c85f1c;
		--wwd-cream: #f6f3ee;
		--wwd-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--wwd-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		background: #fff;
		color: var(--wwd-navy);
		padding: 3.5rem 1.25rem;
	}

	.wwd,
	.wwd *,
	.wwd *::before,
	.wwd *::after {
		box-sizing: border-box;
	}

	.wwd__inner {
		max-width: 1200px;
		margin: 0 auto;
	}

	/* ---------- Header ---------- */
	.wwd__header {
		max-width: 44rem;
		margin-bottom: 2.25rem;
	}

	.wwd__eyebrow {
		margin: 0 0 0.75rem;
		font-family: var(--wwd-body-font);
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: var(--wwd-accent);
	}

	.wwd__heading {
		margin: 0;
		font-family: var(--wwd-display-font);
		font-size: clamp(2rem, 8vw, 2.5rem);
		font-weight: 700;
		line-height: 1.05;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.wwd__intro {
		margin: 1rem 0 0;
		font-family: var(--wwd-body-font);
		font-size: 1.05rem;
		line-height: 1.6;
		color: rgb(14 30 60 / 0.8);
	}

	/* ---------- Cards ---------- */
	.cards {
		display: grid;
		gap: 1rem;
		margin: 0;
		padding: 0;
		list-style: none;
	}

	.card {
		display: flex;
		flex-direction: column;
		overflow: hidden;
		border-radius: 8px;
		border-top: 4px solid var(--wwd-accent);
		background: var(--wwd-cream);
		font-family: var(--wwd-body-font);
	}

	.card.has-image {
		border-top: 0;
	}

	.card__img {
		display: block;
		width: 100%;
		aspect-ratio: 4 / 3;
		object-fit: cover;
	}

	.card__icon {
		display: grid;
		place-items: center;
		width: 3.25rem;
		height: 3.25rem;
		margin: 1.5rem 1.5rem 0;
		border-radius: 50%;
		background: var(--wwd-navy);
	}

	.card__icon svg {
		width: 1.6rem;
		height: 1.6rem;
		fill: none;
		stroke: #fff;
		stroke-width: 2;
		stroke-linecap: round;
		stroke-linejoin: round;
	}

	.card__body {
		display: flex;
		flex: 1;
		flex-direction: column;
		padding: 1.25rem 1.5rem 1.5rem;
	}

	.card__title {
		margin: 0;
		font-family: var(--wwd-display-font);
		font-size: 1.5rem;
		font-weight: 700;
		letter-spacing: 0.02em;
		line-height: 1.1;
		text-transform: uppercase;
	}

	.card__text {
		margin: 0.6rem 0 0;
		font-size: 1rem;
		line-height: 1.6;
		color: rgb(14 30 60 / 0.8);
	}

	.card__link {
		align-self: flex-start;
		margin-top: auto;
		padding-top: 1rem;
		font-weight: 600;
		color: var(--wwd-blue);
		text-decoration: none;
	}

	.card__link:focus-visible {
		outline: 3px solid var(--wwd-accent);
		outline-offset: 3px;
		border-radius: 2px;
	}

	@media (hover: hover) {
		.card__link:hover {
			text-decoration: underline;
			text-underline-offset: 3px;
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

	/* ---------- Desktop ---------- */
	@media (min-width: 900px) {
		.wwd {
			padding: 5.5rem 2rem;
		}

		.wwd__header {
			margin-bottom: 3rem;
		}

		.wwd__eyebrow {
			font-size: 0.875rem;
		}

		.wwd__heading {
			font-size: clamp(2.75rem, 4.5vw, 3.75rem);
		}

		.wwd__intro {
			font-size: 1.15rem;
		}

		.cards {
			grid-template-columns: repeat(3, 1fr);
			gap: 1.5rem;
		}

		.card__icon {
			margin: 2rem 2rem 0;
		}

		.card__body {
			padding: 1.5rem 2rem 2rem;
		}
	}
</style>