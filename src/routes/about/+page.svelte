<script lang="ts">
	import { replaceState } from '$app/navigation';
	import PageBanner from '../../lib/components/PageBanner.svelte';
	import Scripture from '../../lib/components/Scripture.svelte';
	import ServiceTimes from '../../lib/components/ServiceTimes.svelte';

	const sections = [
		{ id: 'mission', label: 'Our mission' },
		{ id: 'how-we-serve', label: 'How we serve' },
		{ id: 'where-we-serve', label: 'Where we serve' },
		{ id: 'our-focus', label: 'Our focus' },
		{ id: 'what-we-believe', label: 'What we believe' }
	];

	const focus = [
		{
			title: 'The lost and the least',
			text: 'We focus on ministry to the most poor, oppressed, homeless, hopeless, addicted and outcast: the sheep lost in the deepest darkness.'
		},
		{
			title: 'Church without walls',
			text: 'Rarely do the poorest, most marginalized people have access to the Gospel through traditional church structures. We work to break through the confines of poverty, homelessness, addiction and mental illness to bring the healing hope and light of Jesus Christ to them. We operate outside conventional church norms in order to reach the fringe.'
		},
		{
			title: 'A local mission field',
			text: 'God uses 1000 Hills church services as mission opportunities for local church communities. We provide volunteer and service programs where congregations can take part in discipleship and mission work, and experience a real mission field without leaving the country.'
		},
		{
			title: 'Partnership through the local church',
			text: "Our community's churches are God's provision for completing His work, and we partner with many local churches and community ministries. We do not rely on, accept or operate with any government or municipal agency support. Instead, we rely on Christ's Church of believers to enable God's work to be done."
		}
	];

	const beliefs = [
		'the Bible is the inspired, written Word of God, the fully true, authoritative and infallible writings for what we believe. We believe the Bible tells us how to live as the Body of Christ.',
		'in one God, eternally existing in three persons: Father, Son, and Holy Spirit. We believe in the Holy Trinity.',
		'in the deity of our Lord Jesus Christ. We believe Jesus Christ is the Son of God, the Messiah promised in the Old Testament. We believe He came to earth in His virgin birth, in His sinless life, in His miracles. Ultimately, we believe in His atoning Blood through His death on the cross, His burial for 3 days and nights, in His resurrection and in His ascension to the right hand of God the Father with the promise of His return in power and glory.',
		'that faith in our Lord Jesus Christ is the way to salvation for lost and sinful mankind.',
		'the Holy Spirit is the very Spirit of God who lives inside of all believers.',
		'humans were created in the image of God and are the object of His love, yet because of sin are eternally separated from God and His perfect plan.',
		'in the forgiveness of sins, the resurrection of the body, and eternal life.',
		'all who place their trust in the Lord Jesus Christ are saved from eternal separation from God and are adopted as children of God to spend all of eternity with Him.'
	];

	// ---------- Tabs ----------
	const ids = sections.map((s) => s.id);
	let active = $state(sections[0].id);
	let tabList = $state<HTMLElement>();
	let anchor = $state<HTMLElement>();

	const headerHeight = () => (window.matchMedia('(min-width: 900px)').matches ? 84 : 72);
	const reducedMotion = () => window.matchMedia('(prefers-reduced-motion: reduce)').matches;

	/** Scroll back up to the tab bar if the reader is further down the page */
	function scrollToTabs(force = false) {
		if (!anchor) return;
		const top = anchor.getBoundingClientRect().top + window.scrollY - headerHeight();
		if (force || window.scrollY > top) {
			window.scrollTo({ top, behavior: reducedMotion() ? 'auto' : 'smooth' });
		}
	}

	function select(id: string, focus = false) {
		active = id;
		replaceState(`#${id}`, {});
		scrollToTabs();
		if (focus) document.getElementById(`tab-${id}`)?.focus();
	}

	// Arrow keys, Home and End move between tabs (standard tab keyboard pattern)
	function onTabKeydown(event: KeyboardEvent) {
		const i = ids.indexOf(active);
		const next =
			event.key === 'ArrowRight' ? (i + 1) % ids.length
			: event.key === 'ArrowLeft' ? (i - 1 + ids.length) % ids.length
			: event.key === 'Home' ? 0
			: event.key === 'End' ? ids.length - 1
			: -1;
		if (next === -1) return;
		event.preventDefault();
		select(ids[next], true);
	}

	// Open the tab named in the URL, e.g. /about#what-we-believe
	$effect(() => {
		const fromHash = (initial = false) => {
			const id = location.hash.slice(1);
			if (ids.includes(id)) {
				active = id;
				if (initial) requestAnimationFrame(() => scrollToTabs(true));
			}
		};
		fromHash(true);
		const onHashChange = () => fromHash();
		window.addEventListener('hashchange', onHashChange);
		return () => window.removeEventListener('hashchange', onHashChange);
	});

	// On phones, keep the selected tab scrolled into view in the horizontal bar
	$effect(() => {
		const tab = tabList?.querySelector<HTMLElement>(`#tab-${active}`);
		if (tabList && tab) {
			tabList.scrollTo({ left: tab.offsetLeft - 16, behavior: 'smooth' });
		}
	});
</script>

<svelte:head>
	<title>About | 1000 Hills Ministry International</title>
	<meta
		name="description"
		content="1000 Hills Ministry serves the poorest of the poor in Houston with daily outdoor church services and a meal at every service. Learn our mission, focus and beliefs."
	/>
</svelte:head>

<PageBanner
	image="/hdr-OutdoorPeople-Blurred.jpg"
	imageAlt="An outdoor church service in a Houston field, with a wooden cross and rows of chairs"
	eyebrow="About us"
	title="Church without walls"
	intro="A Jesus ministry serving Houston's poor and homeless, every day of the year, entirely outdoors."
/>

<div bind:this={anchor} aria-hidden="true"></div>
<div class="page-nav">
	<div
		class="tabs"
		role="tablist"
		aria-label="About 1000 Hills"
		tabindex="-1"
		bind:this={tabList}
		onkeydown={onTabKeydown}
	>
		{#each sections as s (s.id)}
			<button
				type="button"
				role="tab"
				id="tab-{s.id}"
				aria-selected={active === s.id}
				aria-controls="panel-{s.id}"
				tabindex={active === s.id ? 0 : -1}
				onclick={() => select(s.id)}
			>
				{s.label}
			</button>
		{/each}
	</div>
</div>

<div class="panel" role="tabpanel" id="panel-mission" aria-labelledby="tab-mission" tabindex="0" hidden={active !== 'mission'}>
<!-- ============ Mission ============ -->
<section id="mission" class="section">
	<div class="container mission">
		<div>
			<p class="eyebrow">Our mission</p>
			<p class="lead">
				To serve the poorest of the poor, in any community where God leads us to serve.
			</p>
		</div>
		<div class="prose">
			<p>
				Simply put, God leads us to His areas of need, and we go to work seeking and gathering
				the most poverty-stricken people in that area. That could mean pursuing people living
				homeless on the streets, families struggling in generational poverty, or those who have
				lost everything in their addictions.
			</p>
			<p>
				No matter the why or how, no matter the circumstance, we are called to serve God's least
				of these.
			</p>
		</div>
	</div>
</section>

<Scripture
	variant="feature"
	text="Instead, invite the poor, the crippled, the lame, and the blind. Then at the resurrection of the godly, God will reward you for inviting those who could not repay you."
	reference="Luke 14:13–14"
	translation="NLT"
/>
</div>

<div class="panel" role="tabpanel" id="panel-how-we-serve" aria-labelledby="tab-how-we-serve" tabindex="0" hidden={active !== 'how-we-serve'}>
<!-- ============ How we serve ============ -->
<section id="how-we-serve" class="section">
	<div class="container split">
		<div>
			<p class="eyebrow">How we serve</p>
			<h2 class="heading">The Gospel and a meal, every day</h2>
			<div class="prose">
				<p>
					We preach the Gospel of the Lord Jesus Christ and serve meals with His love daily.
				</p>
				<p>
					We serve the homeless population in and around downtown Houston, and families living in
					extreme poverty in Third Ward. Our daily church services include everything you'd expect
					from church: praise and worship, preaching God's Word, and prayer. We serve a meal at
					every service and meet the immediate and pressing needs of our congregation. We just do it
					all outdoors.
				</p>
			</div>
			<p class="statement">We are not primarily a food ministry, but a Jesus ministry.</p>
		</div>

		<aside class="callout" aria-labelledby="disciple-house">
			<p class="callout__eyebrow">Behind every service</p>
			<h3 id="disciple-house" class="callout__title">The Disciple House</h3>
			<p>
				1000 Hills rents a home in Houston for a group of men we call disciples. The Disciple House
				is the headquarters God uses for them to operate the ministry, and the disciples become its
				workers, pastors, evangelists and leaders.
			</p>
			<p>
				Alongside their normal routines, they support every daily service, setting up the chairs,
				tables, speakers and lights for the visiting churches that lead the outdoor services.
			</p>
			<p>
				They are a blessing to the neighborhood, especially to single mothers with children and to
				elderly neighbors living in abandoned houses, who come daily for food, clothing, household
				items and other needs. Together, the disciples help feed thousands of people in Houston every
				week.
			</p>
		</aside>
	</div>
</section>
</div>

<div class="panel" role="tabpanel" id="panel-where-we-serve" aria-labelledby="tab-where-we-serve" tabindex="0" hidden={active !== 'where-we-serve'}>
<!-- ============ Where we serve ============ -->
<ServiceTimes id="where-we-serve" eyebrow="Where we serve" />
</div>

<div class="panel" role="tabpanel" id="panel-our-focus" aria-labelledby="tab-our-focus" tabindex="0" hidden={active !== 'our-focus'}>
<!-- ============ Our focus ============ -->
<section id="our-focus" class="section">
	<div class="container">
		<p class="eyebrow">Our focus</p>
		<h2 class="heading">Going after the one</h2>
		<div class="prose">
			<Scripture
				text="Suppose one of you has a hundred sheep and loses one of them. Does he not leave the ninety-nine in the open country and go after the lost sheep until he finds it? … I tell you that in the same way there will be more rejoicing in heaven over one sinner who repents than over ninety-nine righteous persons who do not need to repent."
				reference="Luke 15:4, 7"
				translation="NIV"
			/>
		</div>

		<ol class="focus">
			{#each focus as item, i (item.title)}
				<li class="focus__item">
					<span class="focus__num" aria-hidden="true">{String(i + 1).padStart(2, '0')}</span>
					<h3 class="focus__title">{item.title}</h3>
					<p>{item.text}</p>
				</li>
			{/each}
		</ol>
	</div>
</section>
</div>

<div class="panel" role="tabpanel" id="panel-what-we-believe" aria-labelledby="tab-what-we-believe" tabindex="0" hidden={active !== 'what-we-believe'}>
<!-- ============ What we believe ============ -->
<section id="what-we-believe" class="section">
	<div class="container">
		<p class="eyebrow">What we believe</p>
		<h2 class="heading">Our statement of faith</h2>

		<div class="gospel">
			<p>
				<strong>We believe</strong> the saving Gospel is depicted by our apostle Paul in
				1 Corinthians 15:1–4.
			</p>
			<p>We are not saved by being good or by our good works.</p>
			<p>
				We are saved by His Grace through our faith in the finished works of the Lord Jesus Christ:
				His death, burial and resurrection. Christ shed His precious Blood for our sins. He died in
				our place. He was our substitute. Trusting in His shed Blood for the forgiveness of our sins
				is what saves us.
			</p>
			<p class="gospel__close">Faith plus nothing!</p>
		</div>

		<ul class="beliefs">
			{#each beliefs as belief (belief)}
				<li><strong>We believe</strong> {belief}</li>
			{/each}
		</ul>
	</div>
</section>
</div>

<style>
	/* Shared tokens, matching the other components */
	.page-nav,
	.section {
		--navy: #0e1e3c;
		--blue: #1f3f75;
		--accent: #c85f1c;
		--cream: #f6f3ee;
		--display-font: var(--font-display, 'Oswald', 'Bebas Neue', Impact, sans-serif);
		--body-font: var(--font-body, 'Inter', system-ui, -apple-system, sans-serif);
	}

	.page-nav *,
	.section,
	.section * {
		box-sizing: border-box;
	}

	/* ---------- On this page bar ---------- */
	.page-nav {
		position: sticky;
		top: 72px; /* height of the site header on phones */
		z-index: 40;
		background: #fff;
		border-bottom: 1px solid rgb(14 30 60 / 0.1);
		box-shadow: 0 2px 10px rgb(14 30 60 / 0.05);
	}

	.tabs {
		display: flex;
		gap: 0.25rem;
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 0.75rem;
		overflow-x: auto;
		scrollbar-width: none;
	}

	.tabs::-webkit-scrollbar {
		display: none;
	}

	.tabs:focus {
		outline: none;
	}

	.tabs button {
		flex: none;
		margin: 0;
		padding: 0.95rem 0.6rem 0.85rem;
		border: 0;
		border-bottom: 3px solid transparent;
		background: none;
		font-family: var(--body-font);
		font-size: 0.9rem;
		font-weight: 600;
		white-space: nowrap;
		color: rgb(14 30 60 / 0.65);
		cursor: pointer;
		transition:
			color 0.2s ease,
			border-color 0.2s ease;
	}

	.tabs button[aria-selected='true'] {
		border-bottom-color: var(--accent);
		color: var(--navy);
	}

	.tabs button:focus-visible {
		outline: 3px solid var(--accent);
		outline-offset: -3px;
	}

	@media (hover: hover) {
		.tabs button:hover {
			color: var(--navy);
		}
	}

	/* Keyboard users land on the panel after the tabs; keep its focus ring subtle */
	.panel:focus-visible {
		outline: 2px solid rgb(200 95 28 / 0.5);
		outline-offset: -2px;
	}

	/* Short tabs still push the footer down a bit, so switching doesn't jump as much */
	.panel {
		min-height: 50vh;
	}

	/* ---------- Sections ---------- */
	.section {
		padding: 3.5rem 1.25rem;
		background: #fff;
		color: var(--navy);
		font-family: var(--body-font);
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
	}

	.eyebrow,
	.callout__eyebrow {
		margin: 0 0 0.75rem;
		font-size: 0.75rem;
		font-weight: 600;
		letter-spacing: 0.14em;
		text-transform: uppercase;
		color: var(--accent);
	}

	.heading {
		margin: 0 0 1.25rem;
		font-family: var(--display-font);
		font-size: clamp(2rem, 8vw, 2.5rem);
		font-weight: 700;
		line-height: 1.05;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.lead {
		max-width: 24ch;
		margin: 0 0 1.5rem;
		font-family: var(--display-font);
		font-size: clamp(1.85rem, 7.5vw, 2.25rem);
		font-weight: 700;
		line-height: 1.1;
		text-transform: uppercase;
		text-wrap: balance;
	}

	.mission .prose p:last-child {
		margin-bottom: 0;
	}

	.prose {
		max-width: 44rem;
	}

	.prose p,
	.callout p,
	.focus__item p,
	.gospel p,
	.beliefs li {
		font-size: 1.05rem;
		line-height: 1.7;
		color: rgb(14 30 60 / 0.85);
	}

	.prose p {
		margin: 0 0 1rem;
	}

	/* ---------- How we serve ---------- */
	.split {
		display: grid;
		gap: 2.5rem;
	}

	.statement {
		max-width: 26ch;
		margin: 1.75rem 0 0;
		padding-left: 1rem;
		border-left: 4px solid var(--accent);
		font-family: var(--display-font);
		font-size: 1.6rem;
		font-weight: 700;
		line-height: 1.15;
		text-transform: uppercase;
		color: var(--accent);
	}

	.callout {
		padding: 1.75rem 1.5rem;
		border-radius: 8px;
		background: var(--navy);
		color: #fff;
	}

	.callout__eyebrow {
		color: #f3b27f;
	}

	.callout__title {
		margin: 0 0 1rem;
		font-family: var(--display-font);
		font-size: 1.75rem;
		font-weight: 700;
		line-height: 1.1;
		text-transform: uppercase;
	}

	.callout p {
		margin: 0 0 0.9rem;
		color: rgb(255 255 255 / 0.85);
	}

	.callout p:last-child {
		margin-bottom: 0;
	}

	/* ---------- Our focus ---------- */
	.focus {
		display: grid;
		gap: 1rem;
		margin: 2rem 0 0;
		padding: 0;
		list-style: none;
	}

	.focus__item {
		padding: 1.5rem;
		border-top: 4px solid var(--accent);
		border-radius: 8px;
		background: var(--cream);
	}

	.focus__num {
		display: block;
		font-family: var(--display-font);
		font-size: 2.25rem;
		font-weight: 700;
		line-height: 1;
		color: var(--accent);
	}

	.focus__title {
		margin: 0.6rem 0 0.5rem;
		font-family: var(--display-font);
		font-size: 1.4rem;
		font-weight: 700;
		line-height: 1.15;
		text-transform: uppercase;
	}

	.focus__item p {
		margin: 0;
	}

	/* ---------- What we believe ---------- */
	.gospel {
		max-width: 46rem;
		padding: 1.5rem;
		border-left: 4px solid var(--blue);
		border-radius: 0 8px 8px 0;
		background: var(--cream);
	}

	.gospel p {
		margin: 0 0 0.75rem;
	}

	.gospel strong,
	.beliefs strong {
		color: var(--navy);
	}

	.gospel__close {
		margin: 1.25rem 0 0 !important;
		font-family: var(--display-font);
		font-size: 1.5rem !important;
		font-weight: 700;
		letter-spacing: 0.02em;
		text-transform: uppercase;
		color: var(--accent) !important;
	}

	.beliefs {
		display: grid;
		margin: 2rem 0 0;
		padding: 0;
		list-style: none;
	}

	.beliefs li {
		padding: 1.1rem 0;
		border-bottom: 1px solid rgb(14 30 60 / 0.12);
	}

	.beliefs li:first-child {
		border-top: 1px solid rgb(14 30 60 / 0.12);
	}

	/* ---------- Desktop ---------- */
	@media (min-width: 900px) {
		.page-nav {
			top: 84px; /* height of the site header on desktop */
		}

		.tabs {
			gap: 1.5rem;
			padding: 0 2rem;
		}

		.tabs button {
			padding-inline: 0;
			font-size: 0.95rem;
		}

		.section {
			padding: 5.5rem 2rem;
		}

		.eyebrow,
		.callout__eyebrow {
			font-size: 0.875rem;
		}

		.heading {
			font-size: clamp(2.75rem, 4.5vw, 3.75rem);
		}

		.mission {
			display: grid;
			grid-template-columns: 1.15fr 1fr;
			align-items: end;
			gap: 4rem;
		}

		.lead {
			margin-bottom: 0;
			font-size: clamp(2.5rem, 3.8vw, 3.25rem);
		}

		.prose p,
		.callout p,
		.gospel p {
			font-size: 1.1rem;
		}

		.split {
			grid-template-columns: 1fr minmax(0, 420px);
			align-items: start;
			gap: 4rem;
		}

		.statement {
			font-size: 1.9rem;
		}

		.callout {
			padding: 2.25rem 2rem;
		}

		.focus {
			grid-template-columns: repeat(2, 1fr);
			gap: 1.5rem;
		}

		.focus__item {
			padding: 2rem;
		}

		.gospel {
			padding: 2rem 2.25rem;
		}

		.beliefs {
			grid-template-columns: repeat(2, 1fr);
			column-gap: 3rem;
		}

		.beliefs li:nth-child(2) {
			border-top: 1px solid rgb(14 30 60 / 0.12);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.tabs button {
			transition: none;
		}
	}
</style>