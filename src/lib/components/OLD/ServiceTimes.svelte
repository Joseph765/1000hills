<script lang="ts">
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
	<div class="services__inner">
		<div class="services__intro">
			{#if eyebrow}
				<p class="services__eyebrow">{eyebrow}</p>
			{/if}
			<h2 id="{id}-heading" class="services__heading">{heading}</h2>
			{#if intro}
				<p class="services__text">{intro}</p>
			{/if}
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
			<p class="card__next" class:live={next?.live} aria-live="polite">
				{#if next}
					<span class="dot" aria-hidden="true"></span>
					{next.text}
				{:else}
					Every day of the week
				{/if}
			</p>

			<dl class="schedule">
				{#each schedule as service (service.label)}
					<div class="schedule__row">
						<dt>{service.label}</dt>
						<dd>{formatTime(service.time)}</dd>
					</div>
				{/each}
			</dl>

			<div class="location">
				<svg class="location__icon" viewBox="0 0 24 24" aria-hidden="true">
					<path
						d="M12 2a7 7 0 0 0-7 7c0 5.25 7 13 7 13s7-7.75 7-13a7 7 0 0 0-7-7Zm0 9.5A2.5 2.5 0 1 1 12 6.5a2.5 2.5 0 0 1 0 5Z"
					/>
				</svg>
				<address>
					<span class="location__name">{locationName}</span>
					{street}<br />
					{cityLine}
				</address>
			</div>

			<a class="directions" href={directionsUrl} target="_blank" rel="noopener noreferrer">
				Get directions
				<span class="sr-only">(opens Google Maps)</span>
			</a>
		</div>
	</div>
</section>

<style>
	.services {
		--svc-navy: #0e1e3c;
		--svc-blue: #1f3f75;
		--svc-accent: #c85f1c;
		--svc-cream: #f6f3ee;
		--svc-display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--svc-body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);

		background: var(--svc-cream);
		color: var(--svc-navy);
		padding: 3.5rem 1.25rem;
	}

	.services,
	.services *,
	.services *::before,
	.services *::after {
		box-sizing: border-box;
	}

	.services__inner {
		display: grid;
		gap: 2rem;
		max-width: 1200px;
		margin: 0 auto;
	}

	/* ---------- Intro ---------- */
	.services__eyebrow {
		margin: 0 0 0.75rem;
		font-family: var(--svc-body-font);
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: var(--svc-accent);
	}

	.services__heading {
		margin: 0;
		font-family: var(--svc-display-font);
		font-size: clamp(2rem, 8vw, 2.5rem);
		font-weight: 700;
		line-height: 1.05;
		text-transform: uppercase;
	}

	.services__text {
		max-width: 46ch;
		margin: 1rem 0 0;
		font-family: var(--svc-body-font);
		font-size: 1.05rem;
		line-height: 1.6;
		color: rgb(14 30 60 / 0.8);
	}

	/* ---------- Optional map ---------- */
	.map {
		overflow: hidden;
		margin-top: 1.75rem;
		aspect-ratio: 4 / 3;
		border-radius: 8px;
		background: #e5e2dc;
		box-shadow: 0 1px 2px rgb(14 30 60 / 0.06);
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
		overflow: hidden;
		border-radius: 8px;
		background: #fff;
		box-shadow:
			0 1px 2px rgb(14 30 60 / 0.06),
			0 12px 32px rgb(14 30 60 / 0.08);
		font-family: var(--svc-body-font);
	}

	.card__next {
		display: flex;
		align-items: center;
		gap: 0.6rem;
		min-height: 3.25rem;
		margin: 0;
		padding: 0.9rem 1.25rem;
		background: var(--svc-navy);
		color: #fff;
		font-size: 0.95rem;
		font-weight: 600;
	}

	.dot {
		flex: none;
		width: 0.55rem;
		height: 0.55rem;
		border-radius: 50%;
		background: #f3b27f;
	}

	.card__next.live .dot {
		background: #4ade80;
		box-shadow: 0 0 0 0 rgb(74 222 128 / 0.6);
		animation: pulse 1.8s ease-out infinite;
	}

	.schedule {
		margin: 0;
		padding: 0.5rem 1.25rem;
	}

	.schedule__row {
		display: flex;
		align-items: baseline;
		justify-content: space-between;
		gap: 1rem;
		padding: 1rem 0;
	}

	.schedule__row + .schedule__row {
		border-top: 1px solid rgb(14 30 60 / 0.1);
	}

	.schedule dt {
		font-size: 1rem;
		font-weight: 600;
	}

	.schedule dd {
		margin: 0;
		font-family: var(--svc-display-font);
		font-size: 1.6rem;
		font-weight: 700;
		letter-spacing: 0.02em;
		white-space: nowrap;
		color: var(--svc-accent);
	}

	.location {
		display: flex;
		gap: 0.75rem;
		margin: 0 1.25rem;
		padding: 1.25rem 0;
		border-top: 1px solid rgb(14 30 60 / 0.1);
	}

	.location__icon {
		flex: none;
		width: 1.4rem;
		height: 1.4rem;
		margin-top: 0.1rem;
		fill: var(--svc-blue);
	}

	address {
		font-style: normal;
		font-size: 1rem;
		line-height: 1.5;
		color: rgb(14 30 60 / 0.85);
	}

	.location__name {
		display: block;
		font-weight: 600;
		color: var(--svc-navy);
	}

	.directions {
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 48px;
		margin: 0 1.25rem 1.25rem;
		border-radius: 4px;
		background: var(--svc-blue);
		color: #fff;
		font-family: var(--svc-display-font);
		font-size: 1.05rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		text-transform: uppercase;
		text-decoration: none;
		transition: background-color 0.2s ease;
	}

	.directions:focus-visible {
		outline: 3px solid var(--svc-accent);
		outline-offset: 3px;
	}

	@media (hover: hover) {
		.directions:hover {
			background: var(--svc-navy);
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
		.services {
			padding: 5.5rem 2rem;
		}

		.services__inner {
			grid-template-columns: 1fr minmax(0, 440px);
			align-items: center;
			gap: 4rem;
		}

		.services__eyebrow {
			font-size: 0.875rem;
		}

		.services__heading {
			font-size: clamp(2.75rem, 4.5vw, 3.75rem);
		}

		.services__text {
			font-size: 1.15rem;
		}
	}

	@keyframes pulse {
		to {
			box-shadow: 0 0 0 0.6rem rgb(74 222 128 / 0);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.card__next.live .dot {
			animation: none;
		}
		.directions {
			transition: none;
		}
	}
</style>