<script>
	import WhatWeDo from '#lib/components/WhatWeDo.svelte';
    import Hero from '../lib/components/Hero.svelte';
    import ServiceTimes from '../lib/components/ServiceTimes.svelte';
    import SeeUsInAction from '../lib/components/SeeUsInAction.svelte';
</script>

<svelte:head>
	<title>1000 Hills Ministry | Feeding Body & Soul in Houston</title>
    <!-- <title>1000 Hills Ministry</title> -->
	<meta name="description" content="Outdoor church services and hot meals for Houston neighbors facing poverty, homelessness and addiction." />
</svelte:head>

<Hero
    eyebrow="1000 Hills Ministry · Houston, Texas"
	headline="Feeding body & soul"
	subhead="Outdoor church services and hot meals for Houston neighbors facing poverty, homelessness and addiction."
	image="/hdr-home2.jpg"
	imageAlt="Hero image"
    imagePosition="42% center"
/>
<ServiceTimes />
<WhatWeDo />
<SeeUsInAction />