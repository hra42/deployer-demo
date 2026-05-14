<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import { Badge } from '$lib/components/ui/badge/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import ThemeToggle from '$lib/components/theme-toggle.svelte';

	import Rocket from '@lucide/svelte/icons/rocket';
	import Paintbrush from '@lucide/svelte/icons/paintbrush';
	import Component from '@lucide/svelte/icons/component';
	import FileCode from '@lucide/svelte/icons/file-code-2';
	import Zap from '@lucide/svelte/icons/zap';
	import MoonStar from '@lucide/svelte/icons/moon-star';
	import ExternalLink from '@lucide/svelte/icons/external-link';
	import ArrowRight from '@lucide/svelte/icons/arrow-right';

	const features = [
		{
			icon: Rocket,
			title: 'SvelteKit',
			description: 'File-based routing, SSR, and a delightful DX powered by Svelte 5 runes.'
		},
		{
			icon: Paintbrush,
			title: 'Tailwind v4',
			description: 'Utility-first styling with the new zero-config engine and CSS variables.'
		},
		{
			icon: Component,
			title: 'shadcn-svelte',
			description: 'Beautiful, accessible components you own — copy-paste, fully themable.'
		},
		{
			icon: FileCode,
			title: 'TypeScript',
			description: 'End-to-end type safety across routes, components, and load functions.'
		},
		{
			icon: Zap,
			title: 'Bun',
			description: 'A fast all-in-one runtime and package manager — install in milliseconds.'
		},
		{
			icon: MoonStar,
			title: 'Dark mode',
			description: 'System-aware theming via mode-watcher with persistent user preference.'
		}
	];
</script>

<header
	class="border-border/60 bg-background/80 sticky top-0 z-50 w-full border-b backdrop-blur-md"
>
	<div class="mx-auto flex h-16 max-w-6xl items-center justify-between px-6">
		<a href="/" class="flex items-center gap-2 font-semibold tracking-tight">
			<span class="bg-foreground text-background grid size-7 place-items-center rounded-md">
				<Rocket class="size-4" />
			</span>
			deployer-demo
		</a>
		<div class="flex items-center gap-2">
			<Button
				variant="ghost"
				size="icon"
				href="https://github.com"
				target="_blank"
				aria-label="Source"
			>
				<ExternalLink class="size-[1.2rem]" />
			</Button>
			<ThemeToggle />
		</div>
	</div>
</header>

<main class="flex-1">
	<section class="relative overflow-hidden">
		<div
			class="bg-foreground/[0.04] pointer-events-none absolute inset-0 -z-10 [mask-image:radial-gradient(ellipse_at_top,black,transparent_70%)]"
		></div>
		<div class="mx-auto flex max-w-3xl flex-col items-center px-6 py-24 text-center sm:py-32">
			<Badge variant="secondary" class="mb-6 gap-1.5">
				<span class="bg-emerald-500 size-1.5 rounded-full"></span>
				v0.1 · Built with SvelteKit
			</Badge>
			<h1
				class="text-foreground text-4xl font-semibold tracking-tight text-balance sm:text-6xl"
			>
				Ship beautiful Svelte apps,
				<span class="text-muted-foreground">faster.</span>
			</h1>
			<p class="text-muted-foreground mt-6 max-w-xl text-lg text-balance">
				A reference setup pairing SvelteKit, Tailwind v4, and shadcn-svelte. Type-safe, themable,
				and ready to deploy.
			</p>
			<div class="mt-10 flex flex-col gap-3 sm:flex-row">
				<Button size="lg">
					Get started
					<ArrowRight class="size-4" />
				</Button>
				<Button variant="outline" size="lg" href="https://shadcn-svelte.com" target="_blank">
					View components
				</Button>
			</div>
		</div>
	</section>

	<section class="border-border/60 border-t">
		<div class="mx-auto max-w-6xl px-6 py-20">
			<div class="mb-12 max-w-2xl">
				<h2 class="text-3xl font-semibold tracking-tight sm:text-4xl">Everything wired up</h2>
				<p class="text-muted-foreground mt-3 text-base">
					Each piece of the stack is configured and demonstrated on this page.
				</p>
			</div>
			<div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
				{#each features as feature (feature.title)}
					{@const Icon = feature.icon}
					<Card.Root class="transition-shadow hover:shadow-md">
						<Card.Header>
							<div
								class="bg-muted text-foreground mb-2 grid size-10 place-items-center rounded-lg"
							>
								<Icon class="size-5" />
							</div>
							<Card.Title>{feature.title}</Card.Title>
							<Card.Description>{feature.description}</Card.Description>
						</Card.Header>
					</Card.Root>
				{/each}
			</div>
		</div>
	</section>
</main>

<footer class="border-border/60 border-t">
	<div
		class="text-muted-foreground mx-auto flex max-w-6xl flex-col items-center justify-between gap-3 px-6 py-8 text-sm sm:flex-row"
	>
		<span>Built with SvelteKit, Tailwind, and shadcn-svelte.</span>
		<div class="flex gap-5">
			<a class="hover:text-foreground transition-colors" href="https://svelte.dev" target="_blank"
				>Svelte</a
			>
			<a
				class="hover:text-foreground transition-colors"
				href="https://tailwindcss.com"
				target="_blank">Tailwind</a
			>
			<a
				class="hover:text-foreground transition-colors"
				href="https://shadcn-svelte.com"
				target="_blank">shadcn-svelte</a
			>
		</div>
	</div>
</footer>
