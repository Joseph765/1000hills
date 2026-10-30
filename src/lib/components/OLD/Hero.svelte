<script lang="ts">
	type Link = { label: string; href: string };

	type Props = {
		image: string;
		imageAlt: string;
		/** CSS object-position for the photo, e.g. "30% center" to keep a subject in frame on phones */
		imagePosition?: string;
		eyebrow?: string;
		headline: string;
		subhead?: string;
		primary?: Link;
		secondary?: Link;
	};

	let {
		image,
		imageAlt,
		imagePosition = 'center 40%',
		eyebrow = 'Houston, Texas',
		headline,
		subhead,
		primary = { label: 'Donate', href: 'https://www.paypal.com/donate/?hosted_button_id=CMTD9CKPZFAJU' },
		secondary = { label: 'Volunteer', href: '/contact' }
	}: Props = $props();
</script>

<section class="hero" aria-labelledby="hero-heading">
	<img
		class="hero__img"
		src={image}
		alt={imageAlt}
		style:object-position={imagePosition}
		fetchpriority="high"
		decoding="async"
	/>

	<div class="hero__content">
		{#if eyebrow}
			<p class="hero__eyebrow">{eyebrow}</p>
		{/if}

		<h1 id="hero-heading" class="hero__headline">{headline}</h1>

		{#if subhead}
			<p class="hero__subhead">{subhead}</p>
		{/if}

		<div class="hero__actions">
			<a class="btn btn--primary" href={primary.href} target="_blank">{primary.label}</a>
			<a class="btn btn--secondary" href={secondary.href}>{secondary.label}</a>
		</div>
	</div>
</section>

<style>
	/* ---------- Base (phones) ---------- */
	.hero {
		--hero-navy: #0e1e3c;
		--hero-accent: #c85f1c;
		--hero-accent-hover: #a94d14;
		--hero-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--hero-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		position: relative;
		isolation: isolate;
		display: grid;
		align-items: end;
		/* svh avoids the iOS address-bar jump that 100vh causes */
		min-height: min(92svh, 680px);
		overflow: hidden;
		color: #fff;
		background: var(--hero-navy);
	}

	/* Keep padding inside widths so content never spills off narrow screens */
	.hero,
	.hero *,
	.hero *::before,
	.hero *::after {
		box-sizing: border-box;
	}

	.hero__img {
		position: absolute;
		inset: 0;
		z-index: -2;
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	/* On phones the text covers most of the photo, so the overlay is stronger */
	.hero::before {
		content: '';
		position: absolute;
		inset: 0;
		z-index: -1;
		background: linear-gradient(
			to top,
			rgb(14 30 60 / 0.95) 0%,
			rgb(14 30 60 / 0.78) 50%,
			rgb(14 30 60 / 0.35) 100%
		);
	}

	.hero__content {
		width: 100%;
		min-width: 0; /* lets the grid item shrink below its content width */
		max-width: 1200px;
		margin: 0 auto;
		padding: 5rem 1.25rem 2.5rem;
		animation: rise 0.7s ease-out both;
	}

	.hero__eyebrow {
		display: flex;
		align-items: center;
		gap: 0.6rem;
		margin: 0 0 0.75rem;
		font-family: var(--hero-body-font);
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		color: #f3b27f;
	}

	/* The accent dash only appears on wider screens, so longer eyebrows fit on one line on phones */
	.hero__eyebrow::before {
		content: '';
		display: none;
		flex: none;
		width: 1.5rem;
		height: 2px;
		background: var(--hero-accent);
	}

	.hero__headline {
		max-width: 16ch;
		margin: 0;
		font-family: var(--hero-display-font);
		font-size: clamp(2.1rem, 9vw, 2.75rem);
		font-weight: 700;
		line-height: 1.04;
		letter-spacing: 0.01em;
		text-transform: uppercase;
		text-wrap: balance;
		overflow-wrap: break-word;
	}

	.hero__subhead {
		max-width: 52ch;
		margin: 1rem 0 0;
		font-family: var(--hero-body-font);
		font-size: 1rem;
		line-height: 1.55;
		color: rgb(255 255 255 / 0.88);
	}

	.hero__actions {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
		margin-top: 1.75rem;
	}

	.btn {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		min-height: 48px; /* comfortable tap target */
		padding: 0.8rem 1.5rem;
		border: 2px solid transparent;
		border-radius: 4px;
		font-family: var(--hero-display-font);
		font-size: 1.05rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		transition:
			background-color 0.2s ease,
			border-color 0.2s ease,
			color 0.2s ease,
			transform 0.2s ease;
	}

	.btn:focus-visible {
		outline: 3px solid #fff;
		outline-offset: 3px;
	}

	.btn--primary {
		background: var(--hero-accent);
		color: #fff;
	}

	.btn--secondary {
		border-color: rgb(255 255 255 / 0.85);
		color: #fff;
	}

	/* Hover effects only on devices that actually hover (avoids sticky hover on touch) */
	@media (hover: hover) {
		.btn:hover {
			transform: translateY(-1px);
		}
		.btn--primary:hover {
			background: var(--hero-accent-hover);
		}
		.btn--secondary:hover {
			background: #fff;
			color: var(--hero-navy);
		}
	}

	/* ---------- Large phones / small tablets ---------- */
	@media (min-width: 560px) {
		.hero__actions {
			flex-direction: row;
			flex-wrap: wrap;
		}
		.btn {
			min-width: 10rem;
		}
	}

	/* ---------- Tablets and up ---------- */
	@media (min-width: 768px) {
		.hero {
			min-height: min(88svh, 760px);
		}

		.hero::before {
			background:
				linear-gradient(
					to top,
					rgb(14 30 60 / 0.92) 0%,
					rgb(14 30 60 / 0.55) 45%,
					rgb(14 30 60 / 0.15) 100%
				),
				linear-gradient(to right, rgb(14 30 60 / 0.55) 0%, transparent 65%);
		}

		.hero__content {
			padding: clamp(6rem, 10vw, 8rem) 2rem clamp(3rem, 6vw, 5rem);
		}

		.hero__eyebrow {
			gap: 0.75rem;
			margin-bottom: 1rem;
			font-size: 0.875rem;
			letter-spacing: 0.14em;
		}

		.hero__eyebrow::before {
			display: block;
			width: 2.5rem;
		}

		.hero__headline {
			max-width: 20ch;
			font-size: clamp(3rem, 5.5vw, 4.5rem);
			line-height: 1;
		}

		.hero__subhead {
			margin-top: 1.5rem;
			font-size: clamp(1.05rem, 1.6vw, 1.25rem);
			line-height: 1.6;
		}

		.hero__actions {
			gap: 0.875rem;
			margin-top: 2.25rem;
		}

		.btn {
			padding: 0.95rem 1.75rem;
			font-size: 1.15rem;
		}
	}

	@keyframes rise {
		from {
			opacity: 0;
			transform: translateY(16px);
		}
		to {
			opacity: 1;
			transform: none;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.hero__content {
			animation: none;
		}
		.btn {
			transition: none;
		}
	}
</style>