<!--
  VideoEmbed
  A responsive 16:9 video frame. Vimeo always embeds directly; YouTube shows
  a lazy poster image with a play button, swapping in the iframe on click
  (so we don't load YouTube's tracking scripts until someone opts in).

  <VideoEmbed provider="vimeo" id="16186350" title="1000 Hills Ministry documentary" />
  <VideoEmbed provider="youtube" id="YOUR_VIDEO_ID" title="A YouTube-hosted video" />
-->
<script lang="ts">
	type Props = {
		provider: 'youtube' | 'vimeo';
		/** The video's ID: youtube.com/watch?v=THIS or vimeo.com/THIS */
		id: string;
		/** Describes the video for screen readers */
		title: string;
	};

	let { provider, id, title }: Props = $props();

	let playing = $state(false);
	let thumbFailed = $state(false);

	const src = $derived(
		provider === 'youtube'
			? `https://www.youtube-nocookie.com/embed/${id}?autoplay=1&rel=0`
			: `https://player.vimeo.com/video/${id}?dnt=1&title=0&byline=0&portrait=0`
	);

	// hqdefault always exists; it's 4:3 with bars, which object-fit: cover crops away
	const thumbnail = $derived(`https://i.ytimg.com/vi/${id}/hqdefault.jpg`);
</script>

<div class="frame">
	{#if provider === 'vimeo' || playing}
		<iframe
			{src}
			{title}
			loading="lazy"
			allow="autoplay; fullscreen; picture-in-picture; encrypted-media"
			allowfullscreen
		></iframe>
	{:else}
		<button type="button" class="poster" onclick={() => (playing = true)}>
			{#if !thumbFailed}
				<img
					src={thumbnail}
					alt=""
					loading="lazy"
					decoding="async"
					onerror={() => (thumbFailed = true)}
				/>
			{/if}
			<span class="play" aria-hidden="true">
				<svg viewBox="0 0 24 24"><path d="M8 5.5v13l11-6.5-11-6.5Z" /></svg>
			</span>
			<span class="visually-hidden">Play video: {title}</span>
		</button>
	{/if}
</div>

<style>
	.frame {
		position: relative;
		overflow: hidden;
		aspect-ratio: 16 / 9;
		border-radius: var(--border-radius);
		background: #000;
		box-shadow: 0 20px 48px rgb(0 0 0 / 0.35);
	}

	iframe,
	.poster {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		border: 0;
	}

	.poster {
		display: grid;
		place-items: center;
		padding: 0;
		background: #000;
		cursor: pointer;
	}

	.poster img {
		position: absolute;
		inset: 0;
		width: 100%;
		max-width: none;
		height: 100%;
		object-fit: cover;
		opacity: 0.85;
		transition: opacity 150ms ease;
	}

	/* Same navy → haze hover as the primary Button variant */
	.play {
		position: relative;
		display: grid;
		place-items: center;
		width: 4.5rem;
		height: 4.5rem;
		border-radius: var(--border-radius-circle);
		background: var(--color-accent);
		box-shadow: 0 6px 20px rgb(0 0 0 / 0.35);
		transition:
			transform 150ms ease,
			background-color 150ms ease;
	}

	.play svg {
		width: 2rem;
		height: 2rem;
		margin-left: 0.2rem; /* optically centers the triangle */
		fill: var(--color-text-on-accent);
		transition: fill 150ms ease;
	}

	.poster:focus-visible {
		outline: 3px solid var(--color-text-link);
		outline-offset: -3px;
	}

	@media (hover: hover) {
		.poster:hover img {
			opacity: 1;
		}
		.poster:hover .play {
			transform: scale(1.06);
			background: var(--color-accent-hover);
		}
		.poster:hover .play svg {
			fill: var(--color-text-on-accent-hover);
		}
	}

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

	@media (prefers-reduced-motion: reduce) {
		.poster img,
		.play,
		.play svg {
			transition: none;
		}
	}
</style>
