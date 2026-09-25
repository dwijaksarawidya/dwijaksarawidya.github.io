<script>
	let { src, alt = '', class: className = '' } = $props();
</script>

<div class="absolute inset-0 z-0 overflow-hidden {className}">
	<img {src} {alt} class="h-full w-full object-cover" />
</div>
