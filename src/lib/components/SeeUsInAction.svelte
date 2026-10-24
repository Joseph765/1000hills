<script lang="ts">
	import VideoEmbed from './VideoEmbed.svelte';

	type Props = {
		id?: string;
		provider?: 'youtube' | 'vimeo';
		/** The video's ID: vimeo.com/THIS or youtube.com/watch?v=THIS */
		videoId?: string;
		/** Describes the video for screen readers */
		videoTitle?: string;
		eyebrow?: string;
		heading?: string;
		text?: string;
		/** Small line under the video, e.g. length */
		caption?: string;
		/** Who produced the video, credited under it with a link */
		creditName?: string;
		creditUrl?: string;
	};

	let {
		id = 'see-us-in-action',
		provider = 'vimeo',
		videoId = '16186350',
		videoTitle = '1000 Hills Ministry International documentary',
		eyebrow = 'See us in action',
		heading = 'The ministry, up close',
		text = 'A short documentary about 1000 Hills Ministry and the Houston neighbors we serve.',
		caption = 'Documentary · 10 min',
		creditName = 'Homer Films',
		creditUrl = 'https://www.homerfilms.com'
	}: Props = $props();
</script>

<section class="video" {id} aria-labelledby="{id}-heading">
	<div class="video__inner">
		<header class="video__header">
			{#if eyebrow}
				<p class="video__eyebrow">{eyebrow}</p>
			{/if}
			<h2 id="{id}-heading" class="video__heading">{heading}</h2>
			{#if text}
				<p class="video__text">{text}</p>
			{/if}
		</header>

		<figure class="player">
			<VideoEmbed {provider} id={videoId} title={videoTitle} />
			{#if caption || creditName}
				<figcaption class="player__caption">
					{#if caption}<span>{caption}</span>{/if}
					{#if creditName}
						<span class="player__credit">
							Thank you to
							{#if creditUrl}
								<a href={creditUrl} target="_blank" rel="noopener noreferrer"
									>{creditName}<span class="sr-only"> (opens in a new tab)</span></a
								>
							{:else}
								{creditName}
							{/if}
							for producing it!
						</span>
					{/if}
				</figcaption>
			{/if}
		</figure>
	</div>
</section>

<style>
	.video {
		--vid-navy: #0e1e3c;
		--vid-accent: #c85f1c;
		--vid-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--vid-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		background: var(--vid-navy);
		color: #fff;
		padding: 3.5rem 1.25rem;
	}

	.video,
	.video *,
	.video *::before,
	.video *::after {
		box-sizing: border-box;
	}

	.video__inner {
		max-width: 960px;
		margin: 0 auto;
	}

	/* ---------- Header ---------- */
	.video__header {
		margin-bottom: 2rem;
		text-align: center;
	}

	.video__eyebrow {
		margin: 0 0 0.75rem;
		font-family: var(--vid-body-font);
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: #f3b27f;
	}

	.video__heading {
		margin: 0;
		font-family: var(--vid-display-font);
		font-size: clamp(2rem, 8vw, 2.5rem);
		font-weight: 700;
		line-height: 1.05;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.video__text {
		max-width: 40rem;
		margin: 1rem auto 0;
		font-family: var(--vid-body-font);
		font-size: 1.05rem;
		line-height: 1.6;
		color: rgb(255 255 255 / 0.8);
	}

	/* ---------- Player ---------- */
	.player {
		margin: 0;
	}

	.player__caption {
		display: flex;
		flex-direction: column;
		gap: 0.3rem;
		margin-top: 0.85rem;
		font-family: var(--vid-body-font);
		font-size: 0.875rem;
		text-align: center;
		color: rgb(255 255 255 / 0.6);
	}

	.player__credit a {
		color: #f3b27f;
		font-weight: 600;
		text-decoration: underline;
		text-decoration-color: rgb(243 178 127 / 0.4);
		text-underline-offset: 3px;
	}

	.player__credit a:focus-visible {
		outline: 3px solid #f3b27f;
		outline-offset: 2px;
		border-radius: 2px;
	}

	@media (hover: hover) {
		.player__credit a:hover {
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

	/* ---------- Desktop ---------- */
	@media (min-width: 900px) {
		.video {
			padding: 5.5rem 2rem;
		}

		.video__header {
			margin-bottom: 2.75rem;
		}

		.video__eyebrow {
			font-size: 0.875rem;
		}

		.video__heading {
			font-size: clamp(2.75rem, 4.5vw, 3.75rem);
		}

		.video__text {
			font-size: 1.15rem;
		}
	}
</style>