<script lang="ts">
	import favicon from '/icon-192.png';
	import '../styling.css';
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}
