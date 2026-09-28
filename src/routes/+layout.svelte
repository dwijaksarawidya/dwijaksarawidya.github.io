<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import logo from '$lib/assets/logo2.jpeg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={logo} />

	<meta
		name="description"
		content="Dengan hormat, kami mengundang Anda untuk menghadiri pernikahan kami."
	/>

	<!-- Open Graph -->
	<meta property="og:type" content="website" />
	<meta property="og:title" content="Undangan Pernikahan | Dwijaksara & Widya" />
	<meta
		property="og:description"
		content="Dengan hormat, kami mengundang Anda untuk menghadiri pernikahan kami."
	/>
	<meta property="og:image" content="https://dwijaksarawidya.github.io/images/galeriFace.jpeg" />
	<meta property="og:image:width" content="1200" />
	<meta property="og:image:height" content="630" />

	<meta property="og:url" content="https://dwijaksarawidya.github.io/" />
	<meta property="og:site_name" content="Undangan Pernikahan" />
</svelte:head>
<div class="mx-auto w-full max-w-[430px] font-body text-[9px] tracking-[3px]">
	{@render children()}
</div>
