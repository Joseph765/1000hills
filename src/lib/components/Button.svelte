<!--
  Button
  Renders a link (<a>) when you pass href, otherwise a real <button>.
  Use a link for navigation ("Give today" → /give) and a button for
  actions on the page (opening a menu, submitting a form).

  <Button href="/give">Give today</Button>
  <Button href="/volunteer" variant="secondary">Volunteer</Button>
  <Button href="/give" variant="inverse">Give today</Button>          (on navy)
  <Button href="/give/monthly" variant="outline-inverse">Give monthly</Button>  (on navy)
  <Button type="submit">Send</Button>
  <Button href="/give" size="s">Give</Button>                          (nav size)
-->
<script lang="ts">
	import type { Snippet } from 'svelte';
	import type { HTMLAnchorAttributes, HTMLButtonAttributes } from 'svelte/elements';

	type Variant = 'primary' | 'secondary' | 'inverse' | 'outline-inverse';
	type Size = 's' | 'm';

	interface Common {
		/** primary: navy. secondary: white with navy outline. inverse / outline-inverse: for navy backgrounds. */
		variant?: Variant;
		size?: Size;
		/** Stretch to the full width of its container. 'mobile' only stretches on small screens. */
		block?: boolean | 'mobile';
		children: Snippet;
	}

	type Props = Common &
		((HTMLAnchorAttributes & { href: string }) | (HTMLButtonAttributes & { href?: undefined }));

	let {
		variant = 'primary',
		size = 'm',
		block = false,
		class: className = '',
		children,
		...rest
	}: Props = $props();

	const classes = $derived(
		`btn ${variant} size-${size} ${block === true ? 'block' : block === 'mobile' ? 'block-mobile' : ''} ${className}`
	);
</script>

{#if rest.href !== undefined}
	<a class={classes} {...rest as HTMLAnchorAttributes}>
		{@render children()}
	</a>
{:else}
	<button class={classes} type="button" {...rest as HTMLButtonAttributes}>
		{@render children()}
	</button>
{/if}

<style>
	.btn {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		gap: var(--space-xs);
		box-sizing: border-box;
		font-family: var(--font-family);
		font-weight: var(--font-weight-active);
		line-height: 1.2;
		text-align: center;
		text-decoration: none;
		border: var(--border-width-strong) solid transparent;
		border-radius: var(--border-radius);
		cursor: pointer;
		transition:
			background-color 150ms ease,
			border-color 150ms ease,
			color 150ms ease;
	}

	/* Sizes (min-height keeps tap targets at 44px or more) */
	.size-m {
		font-size: var(--font-size-m);
		padding: calc(var(--space-m) - var(--border-width-strong)) calc(var(--space-xl) - var(--border-width-strong));
		min-height: 3.25rem;
	}
	.size-s {
		font-size: var(--font-size-s);
		padding: calc(var(--space-s) - var(--border-width-strong)) calc(var(--space-l) - var(--border-width-strong));
		min-height: 2.75rem;
	}

	/* Variants */
	.primary {
		background: var(--color-accent);
		border-color: var(--color-accent);
		color: var(--color-text-on-accent);
	}
	.primary:hover {
		background: var(--color-accent-hover);
		border-color: var(--color-accent-hover);
		color: var(--color-text-on-accent);
	}

	.secondary {
		background: var(--color-surface);
		border-color: var(--color-border-strong);
		color: var(--color-text);
	}
	.secondary:hover {
		background: var(--color-surface-tint);
		color: var(--color-text);
	}

	.inverse {
		background: var(--color-accent-inverse);
		border-color: var(--color-accent-inverse);
		color: var(--color-accent);
	}
	.inverse:hover {
		background: var(--color-surface-tint);
		border-color: var(--color-surface-tint);
		color: var(--color-accent);
	}

	.outline-inverse {
		background: transparent;
		border-color: var(--color-text-on-accent);
		color: var(--color-text-on-accent);
	}
	.outline-inverse:hover {
		background: rgb(255 255 255 / 0.1);
		color: var(--color-text-on-accent);
	}

	/* Keyboard focus */
	.btn:focus-visible {
		outline: 3px solid var(--color-text-link);
		outline-offset: 3px;
	}
	.inverse:focus-visible,
	.outline-inverse:focus-visible {
		outline-color: var(--color-text-on-accent);
	}

	/* Width */
	.block {
		display: flex;
		width: 100%;
	}
	@media (max-width: 480px) {
		.block-mobile {
			display: flex;
			width: 100%;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.btn {
			transition: none;
		}
	}
</style>
