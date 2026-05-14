<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import { ModeWatcher } from 'mode-watcher';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>deployer-demo</title>
	<meta name="description" content="SvelteKit + Tailwind + shadcn-svelte demo" />
</svelte:head>

<ModeWatcher />

<div class="bg-background text-foreground flex min-h-screen flex-col">
	{@render children()}
</div>
