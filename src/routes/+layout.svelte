<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import { ModeWatcher } from 'mode-watcher';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>deployer — ship a container to one host in one command</title>
	<meta
		name="description"
		content="A small Go CLI that deploys a containerised app to a single host: SSH, Docker Compose, Traefik, Cloudflare DNS, and Zero Trust in one command."
	/>
</svelte:head>

<ModeWatcher />

<div class="bg-background text-foreground flex min-h-screen flex-col">
	{@render children()}
</div>
