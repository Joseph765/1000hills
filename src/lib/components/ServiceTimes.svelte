<!--
  ServiceTimes
  Schedule, live/next-service indicator, location and an optional map.
  Used on both /contact and (once migrated) /service.

  <ServiceTimes />
  <ServiceTimes id="our-church" eyebrow="Our church" showMap />
-->
<script lang="ts">
	import Heading from './Heading.svelte';
	import Text from './Text.svelte';
	import Button from './Button.svelte';

	type Service = {
		/** What visitors read, e.g. "Monday – Saturday" */
		label: string;
		/** Days this time applies to: 0 = Sunday … 6 = Saturday */
		days: number[];
		/** 24-hour start time, "HH:MM" */
		time: string;
	};

	type Props = {
		id?: string;
		eyebrow?: string;
		heading?: string;
		intro?: string;
		locationName?: string;
		street?: string;
		cityLine?: string;
		schedule?: Service[];
		/** Minutes after the start time to show "Happening now" */
		durationMinutes?: number;
		timeZone?: string;
		/** Show an embedded Google Map under the intro text */
		showMap?: boolean;
	};

	let {
		id = 'services',
		eyebrow = 'Join us',
		heading = 'Church in the Field',
		intro = 'An outdoor worship service held seven days a week on the big vacant lot on St. Charles Street. Everyone is welcome.',
		locationName = 'The big vacant lot',
		street = '2800 St. Charles St.',
		cityLine = 'Houston, TX 77004',
		schedule = [
			{ label: 'Monday – Saturday', days: [1, 2, 3, 4, 5, 6], time: '19:30' },
			{ label: 'Sunday', days: [0], time: '15:00' }
		],
		durationMinutes = 60,
		timeZone = 'America/Chicago',
		showMap = false
	}: Props = $props();

	const directionsUrl = $derived(
		`https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(`${street}, ${cityLine}`)}`
	);

	// Google's keyless embed: no API key or account needed
	const mapUrl = $derived(
		`https://maps.google.com/maps?q=${encodeURIComponent(`${street}, ${cityLine}`)}&z=15&output=embed`
	);

	const WEEKDAYS = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

	function toMinutes(time: string) {
		const [h, m] = time.split(':').map(Number);
		return h * 60 + m;
	}

	function formatTime(time: string) {
		const [h, m] = time.split(':').map(Number);
		const suffix = h >= 12 ? 'PM' : 'AM';
		const hour = h % 12 || 12;
		return `${hour}:${String(m).padStart(2, '0')} ${suffix}`;
	}

	/** Current weekday and minutes since midnight in the ministry's time zone */
	function nowInZone() {
		const parts = new Intl.DateTimeFormat('en-US', {
			timeZone,
			weekday: 'short',
			hour: 'numeric',
			minute: 'numeric',
			hourCycle: 'h23'
		}).formatToParts(new Date());

		const get = (type: string) => parts.find((p) => p.type === type)?.value ?? '0';
		const day = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'].indexOf(get('weekday'));
		return { day, minutes: Number(get('hour')) * 60 + Number(get('minute')) };
	}

	function startTimeFor(day: number) {
		return schedule.find((s) => s.days.includes(day))?.time;
	}

	function computeNext(): { live: boolean; text: string } | null {
		const now = nowInZone();

		// Is a service going on right now?
		const today = startTimeFor(now.day);
		if (today) {
			const start = toMinutes(today);
			if (now.minutes >= start && now.minutes < start + durationMinutes) {
				return { live: true, text: `Happening now · started at ${formatTime(today)}` };
			}
		}

		// Otherwise find the next start in the coming week
		for (let offset = 0; offset < 8; offset++) {
			const day = (now.day + offset) % 7;
			const time = startTimeFor(day);
			if (!time) continue;
			if (offset === 0 && toMinutes(time) <= now.minutes) continue;

			const when =
				offset === 0
					? toMinutes(time) >= 17 * 60
						? 'Tonight'
						: 'Today'
					: offset === 1
						? 'Tomorrow'
						: WEEKDAYS[day];

			return { live: false, text: `Next service: ${when} at ${formatTime(time)}` };
		}

		return null;
	}

	// Calculated only in the browser so the server's clock never shows a stale time
	let next = $state<{ live: boolean; text: string } | null>(null);

	$effect(() => {
		next = computeNext();
		const timer = setInterval(() => (next = computeNext()), 60_000);
		return () => clearInterval(timer);
	});
</script>

<section class="services" {id} aria-labelledby="{id}-heading">
	<div class="services-inner">
		<div class="services-copy">
			{#if eyebrow}<Text variant="eyebrow">{eyebrow}</Text>{/if}
			<Heading level={2} id="{id}-heading">{heading}</Heading>
			{#if intro}<Text size="l">{intro}</Text>{/if}

			{#if showMap}
				<div class="map">
					<iframe
						src={mapUrl}
						title="Map of {street}, {cityLine}"
						loading="lazy"
						referrerpolicy="no-referrer-when-downgrade"
					></iframe>
				</div>
			{/if}
		</div>

		<div class="card">
			<p class="card-next" class:live={next?.live} aria-live="polite">
				<span class="dot" aria-hidden="true"></span>
				{next ? next.text : 'Every day of the week'}
			</p>

			<div class="card-body">
				<dl class="schedule">
					{#each schedule as service (service.label)}
						<div class="schedule-row">
							<dt>{service.label}</dt>
							<dd>{formatTime(service.time)}</dd>
						</div>
					{/each}
				</dl>

				<div class="location">
					<svg class="location-icon" viewBox="0 0 24 24" aria-hidden="true">
						<path
							d="M12 2a7 7 0 0 0-7 7c0 5.25 7 13 7 13s7-7.75 7-13a7 7 0 0 0-7-7Zm0 9.5A2.5 2.5 0 1 1 12 6.5a2.5 2.5 0 0 1 0 5Z"
						/>
					</svg>
					<address>
						<span class="location-name">{locationName}</span>
						{street}<br />
						{cityLine}
					</address>
				</div>

				<Button href={directionsUrl} target="_blank" rel="noopener noreferrer" block>
					Get directions<span class="visually-hidden"> (opens Google Maps)</span>
				</Button>
			</div>
		</div>
	</div>
</section>

<style>
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

	.services {
		background: var(--color-background);
		padding: var(--space-xxl) var(--size-gutter);
	}

	.services-inner {
		max-width: var(--size-content-max);
		margin-inline: auto;
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: var(--space-xxl) 3.5rem;
	}

	.services-copy {
		flex: 1 1 400px;
		min-width: 0;
		display: flex;
		flex-direction: column;
		gap: var(--space-s);
	}

	.map {
		overflow: hidden;
		margin-top: var(--space-s);
		aspect-ratio: 4 / 3;
		border-radius: var(--border-radius);
		background: var(--color-surface-placeholder);
	}

	.map iframe {
		display: block;
		width: 100%;
		height: 100%;
		border: 0;
	}

	@media (min-width: 900px) {
		.map {
			aspect-ratio: 16 / 9;
		}
	}

	/* ---------- Card ---------- */
	.card {
		flex: 1 1 360px;
		min-width: 0;
		overflow: hidden;
		background: var(--color-surface);
		border-radius: var(--border-radius);
		box-shadow: 0 12px 32px rgb(0 0 0 / 0.08);
	}

	.card-next {
		display: flex;
		align-items: center;
		gap: var(--space-xs);
		min-height: 3.25rem;
		margin: 0;
		padding: var(--space-m) var(--space-l);
		background: var(--color-surface-accent);
		color: var(--color-text-on-accent);
		font-family: var(--font-family);
		font-size: var(--font-size-s);
		font-weight: var(--font-weight-strong);
	}

	.dot {
		flex: none;
		width: 0.55rem;
		height: 0.55rem;
		border-radius: var(--border-radius-circle);
		background: var(--color-accent-hover);
	}

	.card-next.live .dot {
		background: var(--color-status-live);
		box-shadow: 0 0 0 0 rgb(74 222 128 / 0.6);
		animation: pulse 1.8s ease-out infinite;
	}

	.card-body {
		padding: var(--space-l);
		display: flex;
		flex-direction: column;
		gap: var(--space-m);
	}

	.schedule {
		margin: 0;
	}

	.schedule-row {
		display: flex;
		align-items: baseline;
		justify-content: space-between;
		gap: var(--space-m);
		padding: var(--space-s) 0;
	}

	.schedule-row + .schedule-row {
		border-top: var(--border-width) solid var(--color-border);
	}

	.schedule dt {
		font-family: var(--font-family);
		font-size: var(--font-size-m);
		font-weight: var(--font-weight-strong);
		color: var(--color-text);
	}

	.schedule dd {
		margin: 0;
		font-family: var(--font-family-heading);
		font-size: var(--font-size-xl);
		font-weight: var(--font-weight-heading);
		white-space: nowrap;
		color: var(--color-text-link);
	}

	.location {
		display: flex;
		gap: var(--space-s);
		padding-top: var(--space-m);
		border-top: var(--border-width) solid var(--color-border);
	}

	.location-icon {
		flex: none;
		width: 1.4rem;
		height: 1.4rem;
		margin-top: 0.1rem;
		fill: var(--color-text-link);
	}

	address {
		font-style: normal;
		font-family: var(--font-family);
		font-size: var(--font-size-s);
		line-height: var(--line-height);
		color: var(--color-text-weak);
	}

	.location-name {
		display: block;
		font-weight: var(--font-weight-strong);
		color: var(--color-text);
	}

	@keyframes pulse {
		to {
			box-shadow: 0 0 0 0.6rem rgb(74 222 128 / 0);
		}
	}

	@media (max-width: 900px) {
		.services {
			padding-inline: var(--space-m);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.card-next.live .dot {
			animation: none;
		}
	}
</style>
