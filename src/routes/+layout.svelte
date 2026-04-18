<script lang="ts">
	import '../styling.css';
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href="/icon-192.png" />
</svelte:head>

{@render children()}
