<!--
  HillsDivider
  The layered-hills graphic motif, used between sections of different
  backgrounds (e.g. the mist hero and the white section after it).

  <HillsDivider />                                  (into a white section)
  <HillsDivider groundColor="var(--color-background)" /> (into a mist section)
-->
<script lang="ts">
	interface Props {
		/** The fill of the front (lowest) layer — match the section that follows. */
		groundColor?: string;
	}

	let { groundColor = 'var(--color-surface)' }: Props = $props();
</script>

<svg class="hills" viewBox="0 0 1200 110" preserveAspectRatio="none" aria-hidden="true">
	<path class="hill-far" d="M0 56 C200 20 380 50 560 32 C760 12 960 50 1200 22 L1200 110 L0 110 Z" />
	<path class="hill-middle" d="M0 84 C220 54 420 80 640 64 C860 48 1020 76 1200 62 L1200 110 L0 110 Z" />
	<path class="hill-ground" d="M0 102 C260 84 500 100 760 90 C960 82 1100 96 1200 92 L1200 110 L0 110 Z" style="fill: {groundColor}" />
</svg>

<style>
	.hills {
		display: block;
		width: 100%;
		height: 110px;
		background: var(--color-background);
	}
	.hill-far {
		fill: var(--color-hill-far);
	}
	.hill-middle {
		fill: var(--color-hill-middle);
	}

	@media (max-width: 900px) {
		.hills {
			height: 64px;
		}
	}
</style>
