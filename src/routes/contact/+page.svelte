<script lang="ts">
	import PageBanner from '#lib/components/PageBanner.svelte';
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

<PageBanner
	image="/images/prayer-circle.jpg"
	imagePosition="100% 35%"
	imageAlt="People gathered for an outdoor church service"
	blur={4}
	eyebrow="Contact us"
	title="Get in touch"
	intro="Questions about giving, volunteering, or how you and your group or family can get involved? We'd love to hear from you."
/>

<section class="section" aria-label="Contact information">
	<div class="container">
		<div class="cards">
			<!-- Director -->
			<article class="card card--primary">
				<p class="card__eyebrow">{director.role}</p>
				<h2 class="card__title">{director.name}</h2>
				<ul class="card__details">
					<li><a href={telHref}>{director.phone}</a></li>
					<li><a href="mailto:{director.email}">{director.email}</a></li>
				</ul>
				<div class="card__actions">
					<a class="btn btn--primary" href={telHref}>Call</a>
					<a class="btn btn--secondary" href="mailto:{director.email}">Email</a>
				</div>
			</article>

			<!-- Mailing address -->
			<article class="card">
				<p class="card__eyebrow">Mailing address</p>
				<h2 class="card__title">{mailing.name}</h2>
				<address class="card__address">
					{#each mailing.lines as line (line)}
						{line}<br />
					{/each}
				</address>
			</article>

			<!-- Board -->
			<article class="card">
				<p class="card__eyebrow">Leadership</p>
				<h2 class="card__title">Board of Directors</h2>
				<ul class="board">
					{#each board as member (member.name)}
						<li>
							<span class="board__name">{member.name}</span>
							{#if member.role}<span class="board__role">{member.role}</span>{/if}
						</li>
					{/each}
				</ul>
			</article>
		</div>
	</div>
</section>

<ServiceTimes id="our-church" eyebrow="Our church" showMap />

<style>
	.section {
		--navy: #0e1e3c;
		--blue: #1f3f75;
		--accent: #c85f1c;
		--accent-hover: #a94d14;
		--cream: #f6f3ee;
		--display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		padding: 3rem 1.25rem 3.5rem;
		background: #fff;
		color: var(--navy);
		font-family: var(--body-font);
	}

	.section,
	.section * {
		box-sizing: border-box;
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
	}

	.cards {
		display: grid;
		gap: 1rem;
	}

	/* ---------- Cards ---------- */
	.card {
		padding: 1.75rem 1.5rem;
		border-top: 4px solid var(--accent);
		border-radius: 8px;
		background: var(--cream);
	}

	.card--primary {
		border-top-color: transparent;
		background: var(--navy);
		color: #fff;
	}

	.card__eyebrow {
		margin: 0 0 0.5rem;
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: var(--accent);
	}

	.card--primary .card__eyebrow {
		color: #f3b27f;
	}

	.card__title {
		margin: 0;
		font-family: var(--display-font);
		font-size: 1.75rem;
		font-weight: 700;
		line-height: 1.1;
		text-transform: uppercase;
	}

	.card__details {
		margin: 1rem 0 0;
		padding: 0;
		list-style: none;
		font-size: 1.05rem;
		line-height: 1.7;
	}

	.card__details a {
		color: #fff;
		text-decoration: none;
		overflow-wrap: anywhere;
	}

	.card__actions {
		display: flex;
		flex-wrap: wrap;
		gap: 0.75rem;
		margin-top: 1.5rem;
	}

	.btn {
		display: inline-flex;
		flex: 1 1 8rem;
		align-items: center;
		justify-content: center;
		min-height: 48px;
		padding: 0.7rem 1.25rem;
		border: 2px solid transparent;
		border-radius: 4px;
		font-family: var(--display-font);
		font-size: 1.05rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		transition:
			background-color 0.2s ease,
			color 0.2s ease;
	}

	.btn--primary {
		background: var(--accent);
		color: #fff;
	}

	.btn--secondary {
		border-color: rgb(255 255 255 / 0.85);
		color: #fff;
	}

	.card__address {
		margin-top: 1rem;
		font-size: 1.05rem;
		font-style: normal;
		line-height: 1.7;
		color: rgb(14 30 60 / 0.85);
	}

	.board {
		margin: 1rem 0 0;
		padding: 0;
		list-style: none;
	}

	.board li {
		display: flex;
		flex-wrap: wrap;
		align-items: baseline;
		justify-content: space-between;
		gap: 0 1rem;
		padding: 0.6rem 0;
		border-bottom: 1px solid rgb(14 30 60 / 0.1);
		font-size: 1rem;
	}

	.board li:last-child {
		border-bottom: 0;
	}

	.board__name {
		font-weight: 600;
	}

	.board__role {
		font-size: 0.9rem;
		color: rgb(14 30 60 / 0.65);
	}

	.card a:focus-visible {
		outline: 3px solid #fff;
		outline-offset: 3px;
		border-radius: 2px;
	}

	@media (hover: hover) {
		.card__details a:hover {
			text-decoration: underline;
			text-underline-offset: 3px;
		}
		.btn--primary:hover {
			background: var(--accent-hover);
		}
		.btn--secondary:hover {
			background: #fff;
			color: var(--navy);
		}
	}

	/* ---------- Desktop ---------- */
	@media (min-width: 900px) {
		.section {
			padding: 4.5rem 2rem 5rem;
		}

		.cards {
			grid-template-columns: 1.15fr 1fr 1fr;
			align-items: start;
			gap: 1.5rem;
		}

		.card {
			padding: 2rem;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.btn {
			transition: none;
		}
	}
</style>