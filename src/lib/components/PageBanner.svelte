<script lang="ts">
	type Props = {
		image: string;
		imageAlt?: string;
		/** CSS object-position, e.g. "40% center" to keep the subject in frame on phones */
		imagePosition?: string;
		eyebrow?: string;
		title: string;
		intro?: string;
	};

	let { image, imageAlt = '', imagePosition = 'center', eyebrow, title, intro }: Props = $props();
</script>

<section class="banner">
	<img
		class="banner__img"
		src={image}
		alt={imageAlt}
		style:object-position={imagePosition}
		fetchpriority="high"
		decoding="async"
	/>

	<div class="banner__content">
		{#if eyebrow}
			<p class="banner__eyebrow">{eyebrow}</p>
		{/if}
		<h1 class="banner__title">{title}</h1>
		{#if intro}
			<p class="banner__intro">{intro}</p>
		{/if}
	</div>
</section>

<style>
	.banner {
		--banner-navy: #0e1e3c;
		--banner-accent: #c85f1c;
		--banner-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--banner-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		position: relative;
		isolation: isolate;
		display: grid;
		align-items: end;
		min-height: min(46svh, 360px);
		overflow: hidden;
		color: #fff;
		background: var(--banner-navy);
	}

	.banner,
	.banner *,
	.banner *::before,
	.banner *::after {
		box-sizing: border-box;
	}

	.banner__img {
		position: absolute;
		inset: 0;
		z-index: -2;
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.banner::before {
		content: '';
		position: absolute;
		inset: 0;
		z-index: -1;
		background: linear-gradient(
			to top,
			rgb(14 30 60 / 0.92) 0%,
			rgb(14 30 60 / 0.6) 55%,
			rgb(14 30 60 / 0.35) 100%
		);
	}

	.banner__content {
		width: 100%;
		min-width: 0;
		max-width: 1200px;
		margin: 0 auto;
		padding: 4rem 1.25rem 2.25rem;
	}

	.banner__eyebrow {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		margin: 0 0 0.6rem;
		font-family: var(--banner-body-font);
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: #f3b27f;
	}

	.banner__eyebrow::before {
		content: '';
		flex: none;
		width: 1.5rem;
		height: 2px;
		background: var(--banner-accent);
	}

	.banner__title {
		margin: 0;
		font-family: var(--banner-display-font);
		font-size: clamp(2.25rem, 10vw, 3rem);
		font-weight: 700;
		line-height: 1;
		letter-spacing: 0.01em;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.banner__intro {
		max-width: 44ch;
		margin: 0.85rem 0 0;
		font-family: var(--banner-body-font);
		font-size: 1rem;
		line-height: 1.55;
		color: rgb(255 255 255 / 0.88);
	}

	@media (min-width: 768px) {
		.banner {
			min-height: min(50svh, 440px);
		}

		.banner::before {
			background:
				linear-gradient(to top, rgb(14 30 60 / 0.9) 0%, rgb(14 30 60 / 0.45) 50%, rgb(14 30 60 / 0.15) 100%),
				linear-gradient(to right, rgb(14 30 60 / 0.5) 0%, transparent 60%);
		}

		.banner__content {
			padding: 6rem 2rem 3.25rem;
		}

		.banner__eyebrow {
			font-size: 0.875rem;
		}

		.banner__eyebrow::before {
			width: 2.5rem;
		}

		.banner__title {
			font-size: clamp(3rem, 6vw, 4.5rem);
		}

		.banner__intro {
			font-size: 1.15rem;
		}
	}
</style>