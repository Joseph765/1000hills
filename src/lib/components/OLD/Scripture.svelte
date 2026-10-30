<script lang="ts">
	type Props = {
		/** The verse text, without quotation marks */
		text: string;
		/** e.g. "Luke 14:13–14" */
		reference: string;
		/** e.g. "NLT" or "NIV" — shown after the reference */
		translation?: string;
		variant?: 'feature' | 'inline';
	};

	let { text, reference, translation, variant = 'inline' }: Props = $props();
</script>

{#if variant === 'feature'}
	<section class="scripture scripture--feature">
		<figure>
			<span class="mark" aria-hidden="true">“</span>
			<blockquote><p>{text}</p></blockquote>
			<figcaption>
				<cite>{reference}</cite>{#if translation}<span class="translation">{` · ${translation}`}</span>{/if}
			</figcaption>
		</figure>
	</section>
{:else}
	<figure class="scripture scripture--inline">
		<blockquote><p>{text}</p></blockquote>
		<figcaption>
			<cite>{reference}</cite>{#if translation}<span class="translation">{` · ${translation}`}</span>{/if}
		</figcaption>
	</figure>
{/if}

<style>
	.scripture {
		--scr-navy: #0e1e3c;
		--scr-accent: #c85f1c;
		--scr-cream: #f6f3ee;
		--scr-serif: var(--font-scripture, 'Lora', Georgia, 'Times New Roman', serif);
		--scr-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
	}

	.scripture,
	.scripture * {
		box-sizing: border-box;
	}

	figure,
	blockquote,
	p {
		margin: 0;
	}

	blockquote p {
		font-family: var(--scr-serif);
		font-style: italic;
		text-wrap: pretty;
	}

	cite {
		font-style: normal;
	}

	figcaption {
		font-family: var(--scr-display-font);
		font-weight: 600;
		letter-spacing: 0.08em;
		text-transform: uppercase;
	}

	.translation {
		opacity: 0.75;
	}

	/* ---------- Feature ---------- */
	.scripture--feature {
		position: relative;
		overflow: hidden;
		padding: 3.5rem 1.25rem;
		background: var(--scr-navy);
		color: #fff;
	}

	.scripture--feature figure {
		position: relative;
		max-width: 52rem;
		margin: 0 auto;
		text-align: center;
	}

	.mark {
		display: block;
		height: 3.25rem;
		font-family: var(--scr-serif);
		font-size: 6rem;
		line-height: 1;
		color: var(--scr-accent);
	}

	.scripture--feature blockquote p {
		font-size: clamp(1.3rem, 5vw, 1.6rem);
		line-height: 1.5;
	}

	.scripture--feature figcaption {
		margin-top: 1.5rem;
		font-size: 1rem;
		color: #f3b27f;
	}

	.scripture--feature figcaption::before {
		content: '';
		display: block;
		width: 2.5rem;
		height: 2px;
		margin: 0 auto 1rem;
		background: var(--scr-accent);
	}

	/* ---------- Inline ---------- */
	.scripture--inline {
		margin: 2rem 0;
		padding: 1.5rem 1.5rem 1.4rem;
		border-left: 4px solid var(--scr-accent);
		border-radius: 0 6px 6px 0;
		background: var(--scr-cream);
		color: var(--scr-navy);
	}

	.scripture--inline blockquote p {
		font-size: 1.15rem;
		line-height: 1.6;
	}

	.scripture--inline figcaption {
		margin-top: 0.85rem;
		font-size: 0.9rem;
		color: var(--scr-accent);
	}

	@media (min-width: 900px) {
		.scripture--feature {
			padding: 5.5rem 2rem;
		}

		.mark {
			height: 4rem;
			font-size: 7.5rem;
		}

		.scripture--feature blockquote p {
			font-size: clamp(1.6rem, 2.6vw, 2.1rem);
			line-height: 1.45;
		}

		.scripture--inline {
			padding: 1.75rem 2rem 1.6rem;
		}

		.scripture--inline blockquote p {
			font-size: 1.3rem;
		}
	}
</style>