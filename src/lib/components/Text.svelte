<!--
  Text
  Body copy, eyebrow labels and captions.

  <Text>Default paragraph (17px, weak tone)</Text>
  <Text size="l">Lead paragraph (20px)</Text>
  <Text variant="eyebrow">Church in the Field · Houston, Texas</Text>
  <Text as="figcaption" variant="caption">[Names], at Church in the Field</Text>
-->
<script lang="ts">
	import type { Snippet } from 'svelte';
	import type { HTMLAttributes } from 'svelte/elements';

	type Size = 'xs' | 's' | 'm' | 'l';
	type Tone = 'default' | 'weak' | 'link' | 'on-accent' | 'on-accent-weak';
	type Weight = 'regular' | 'strong' | 'active';
	type Variant = 'body' | 'eyebrow' | 'caption';

	interface Props extends HTMLAttributes<HTMLElement> {
		/** Which HTML element to render. */
		as?: 'p' | 'span' | 'div' | 'small' | 'figcaption' | 'li' | 'strong' | 'em';
		/** Font size, from the --font-size-* tokens. */
		size?: Size;
		/** Text color, from the --color-text-* tokens. */
		tone?: Tone;
		weight?: Weight;
		/** body: normal text. eyebrow: small uppercase label. caption: Lora italic. */
		variant?: Variant;
		/** Limit line length for comfortable reading (on by default for paragraphs). */
		measure?: boolean;
		children: Snippet;
	}

	let {
		as = 'p',
		size,
		tone,
		weight = 'regular',
		variant = 'body',
		measure,
		class: className = '',
		children,
		...rest
	}: Props = $props();

	// Sensible defaults per variant, overridable by props.
	const resolvedSize = $derived(size ?? (variant === 'eyebrow' ? 'xs' : variant === 'caption' ? 's' : 'm'));
	const resolvedTone = $derived(tone ?? (variant === 'eyebrow' ? 'link' : 'weak'));
	const resolvedMeasure = $derived(measure ?? as === 'p');
</script>

<svelte:element
	this={as}
	class="text size-{resolvedSize} tone-{resolvedTone} weight-{weight} variant-{variant} {className}"
	class:measure={resolvedMeasure}
	{...rest}
>
	{@render children()}
</svelte:element>

<style>
	.text {
		margin: 0;
		font-family: var(--font-family);
		line-height: var(--line-height);
	}

	.measure {
		max-width: var(--size-text-max);
	}

	/* Sizes */
	.size-xs { font-size: var(--font-size-xs); }
	.size-s { font-size: var(--font-size-s); }
	.size-m { font-size: var(--font-size-m); }
	.size-l { font-size: var(--font-size-l); }

	/* Tones */
	.tone-default { color: var(--color-text); }
	.tone-weak { color: var(--color-text-weak); }
	.tone-link { color: var(--color-text-link); }
	.tone-on-accent { color: var(--color-text-on-accent); }
	.tone-on-accent-weak { color: var(--color-text-on-accent-weak); }

	/* Weights */
	.weight-regular { font-weight: var(--font-weight); }
	.weight-strong { font-weight: var(--font-weight-strong); }
	.weight-active { font-weight: var(--font-weight-active); }

	/* Variants */
	.variant-eyebrow {
		font-weight: var(--font-weight-active);
		letter-spacing: var(--letter-spacing-caps);
		text-transform: uppercase;
		line-height: 1.4;
	}

	.variant-caption {
		font-family: var(--font-family-heading);
		font-style: italic;
		line-height: 1.5;
	}
</style>
