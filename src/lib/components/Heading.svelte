<!--
  Heading
  Lora headings. The level sets the HTML tag (for accessibility and SEO);
  the size sets how big it looks. They default to matching, but you can
  separate them, e.g. an <h2> that looks like an <h3>.

  <Heading level={1}>No one in Houston should face despair alone.</Heading>
  <Heading level={2}>What happens in the field</Heading>
  <Heading level={3} size="l">A meal to share</Heading>
  <Heading level={2} tone="on-accent">[$ amount] feeds everyone…</Heading>
-->
<script lang="ts">
	import type { Snippet } from 'svelte';
	import type { HTMLAttributes } from 'svelte/elements';

	type Level = 1 | 2 | 3 | 4 | 5 | 6;
	type Size = 'xxxl' | 'xxl' | 'xl' | 'l';
	type Tone = 'default' | 'on-accent';

	interface Props extends HTMLAttributes<HTMLHeadingElement> {
		level?: Level;
		/** Visual size, from the --font-size-* tokens. Defaults by level. */
		size?: Size;
		tone?: Tone;
		/** Balance line lengths in multi-line headings (on by default). */
		balance?: boolean;
		children: Snippet;
	}

	let {
		level = 2,
		size,
		tone = 'default',
		balance = true,
		class: className = '',
		children,
		...rest
	}: Props = $props();

	const sizeByLevel: Record<Level, Size> = { 1: 'xxxl', 2: 'xxl', 3: 'xl', 4: 'l', 5: 'l', 6: 'l' };
	const resolvedSize = $derived(size ?? sizeByLevel[level]);
</script>

<svelte:element
	this={`h${level}`}
	class="heading size-{resolvedSize} tone-{tone} {className}"
	class:balance
	{...rest}
>
	{@render children()}
</svelte:element>

<style>
	.heading {
		margin: 0;
		font-family: var(--font-family-heading);
		font-weight: var(--font-weight-heading);
		line-height: var(--line-height-heading);
	}

	.balance {
		text-wrap: balance;
	}

	.size-xxxl {
		font-size: var(--font-size-xxxl);
		line-height: var(--line-height-tight);
		letter-spacing: var(--letter-spacing-heading);
	}
	.size-xxl { font-size: var(--font-size-xxl); }
	.size-xl { font-size: var(--font-size-xl); }
	.size-l { font-size: var(--font-size-l); }

	.tone-default { color: var(--color-text); }
	.tone-on-accent { color: var(--color-text-on-accent); }
</style>
