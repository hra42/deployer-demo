<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import { Badge } from '$lib/components/ui/badge/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import ThemeToggle from '$lib/components/theme-toggle.svelte';

	import Ship from '@lucide/svelte/icons/ship';
	import GitBranch from '@lucide/svelte/icons/git-branch';
	import FileCheck from '@lucide/svelte/icons/file-check-2';
	import Container from '@lucide/svelte/icons/container';
	import Globe from '@lucide/svelte/icons/globe';
	import ShieldCheck from '@lucide/svelte/icons/shield-check';
	import Terminal from '@lucide/svelte/icons/terminal';
	import ExternalLink from '@lucide/svelte/icons/external-link';
	import ArrowRight from '@lucide/svelte/icons/arrow-right';
	import Copy from '@lucide/svelte/icons/copy';
	import Check from '@lucide/svelte/icons/check';

	const phases = [
		{
			icon: Terminal,
			step: '1',
			title: 'Connect to host',
			description: 'Opens an SSH session to your single deploy host using the key from ~/.deployer.yml.'
		},
		{
			icon: GitBranch,
			step: '2',
			title: 'Sync repo',
			description: 'Clones (or fast-forwards) the target GitHub repo into a domain-slugged path on the host.'
		},
		{
			icon: FileCheck,
			step: '3',
			title: 'Validate compose',
			description: 'Hard-fails fast if Dockerfile or docker-compose.yml is missing from the repo root.'
		},
		{
			icon: Container,
			step: '4',
			title: 'Bring up containers',
			description: 'Runs docker compose up -d with COMPOSE_PROJECT_NAME set to the domain slug.'
		},
		{
			icon: Globe,
			step: '5',
			title: 'Cloudflare DNS',
			description: 'Creates or updates a proxied CNAME from your app domain to the deploy host.'
		},
		{
			icon: ShieldCheck,
			step: '6',
			title: 'Cloudflare Zero Trust',
			description: 'Attaches the new app to a pre-existing Access policy so only your team can reach it.'
		}
	];

	const deployCmd = 'deployer deploy --repo owner/name --domain app.example.com';
	let copied = $state(false);

	function copyDeployCmd() {
		navigator.clipboard.writeText(deployCmd).then(() => {
			copied = true;
			setTimeout(() => (copied = false), 1500);
		});
	}
</script>

<header
	class="border-border/60 bg-background/80 sticky top-0 z-50 w-full border-b backdrop-blur-md"
>
	<div class="mx-auto flex h-16 max-w-6xl items-center justify-between px-6">
		<a href="/" class="flex items-center gap-2 font-semibold tracking-tight">
			<span class="bg-foreground text-background grid size-7 place-items-center rounded-md">
				<Ship class="size-4" />
			</span>
			deployer
		</a>
		<div class="flex items-center gap-2">
			<Button
				variant="ghost"
				size="sm"
				href="https://github.com/hra42/deployer"
				target="_blank"
			>
				GitHub
				<ExternalLink class="size-3.5" />
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
				<span class="size-1.5 rounded-full bg-emerald-500"></span>
				A small Go CLI · One command
			</Badge>
			<h1
				class="text-foreground text-4xl font-semibold tracking-tight text-balance sm:text-6xl"
			>
				Ship a container to one host
				<span class="text-muted-foreground">in one command.</span>
			</h1>
			<p class="text-muted-foreground mt-6 max-w-xl text-lg text-balance">
				Point deployer at a GitHub repo and a domain. It clones the repo over SSH, brings up
				your Compose stack behind Traefik, sets DNS on Cloudflare, and locks it behind a Zero
				Trust policy.
			</p>

			<div class="mt-10 flex w-full max-w-xl flex-col gap-3">
				<button
					type="button"
					onclick={copyDeployCmd}
					class="group border-border bg-muted/40 hover:bg-muted/70 flex items-center justify-between gap-3 rounded-lg border px-4 py-3 text-left font-mono text-sm transition-colors"
				>
					<span class="truncate">
						<span class="text-muted-foreground select-none">$ </span>{deployCmd}
					</span>
					{#if copied}
						<Check class="text-emerald-500 size-4 shrink-0" />
					{:else}
						<Copy class="text-muted-foreground group-hover:text-foreground size-4 shrink-0" />
					{/if}
				</button>
				<div class="flex flex-col gap-2 sm:flex-row">
					<Button size="lg" href="https://github.com/hra42/deployer#install" target="_blank">
						Install
						<ArrowRight class="size-4" />
					</Button>
					<Button
						variant="outline"
						size="lg"
						href="https://github.com/hra42/deployer"
						target="_blank"
					>
						Read the docs
					</Button>
				</div>
			</div>
		</div>
	</section>

	<section class="border-border/60 border-t">
		<div class="mx-auto max-w-6xl px-6 py-20">
			<div class="mb-12 max-w-2xl">
				<h2 class="text-3xl font-semibold tracking-tight sm:text-4xl">
					Six phases, one summary
				</h2>
				<p class="text-muted-foreground mt-3 text-base">
					Every deploy runs the same pipeline. If a phase fails, the summary still prints — so
					you can see at a glance what ran, what was skipped, and what broke.
				</p>
			</div>
			<div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
				{#each phases as phase (phase.step)}
					{@const Icon = phase.icon}
					<Card.Root class="transition-shadow hover:shadow-md">
						<Card.Header>
							<div class="mb-2 flex items-center justify-between">
								<div
									class="bg-muted text-foreground grid size-10 place-items-center rounded-lg"
								>
									<Icon class="size-5" />
								</div>
								<span class="text-muted-foreground font-mono text-xs">
									[{phase.step}/6]
								</span>
							</div>
							<Card.Title>{phase.title}</Card.Title>
							<Card.Description>{phase.description}</Card.Description>
						</Card.Header>
					</Card.Root>
				{/each}
			</div>
		</div>
	</section>

	<section class="border-border/60 border-t">
		<div class="mx-auto max-w-3xl px-6 py-20">
			<h2 class="text-3xl font-semibold tracking-tight sm:text-4xl">Built for one host</h2>
			<p class="text-muted-foreground mt-3 text-base">
				No control plane, no agents, no per-app YAML scattered across machines. The boring
				config — SSH host, key path, GitHub token, Cloudflare credentials — lives once in
				<code class="bg-muted rounded px-1.5 py-0.5 font-mono text-sm">~/.deployer.yml</code>.
				Run <code class="bg-muted rounded px-1.5 py-0.5 font-mono text-sm">deployer setup</code>
				once; reuse it for every deploy.
			</p>
			<p class="text-muted-foreground mt-3 text-base">
				This very page is the canonical test target — a SvelteKit app with
				<code class="bg-muted rounded px-1.5 py-0.5 font-mono text-sm">adapter-node</code>, a
				<code class="bg-muted rounded px-1.5 py-0.5 font-mono text-sm">Dockerfile</code>, and a
				Traefik-ready
				<code class="bg-muted rounded px-1.5 py-0.5 font-mono text-sm">docker-compose.yml</code>.
				If deployer ships, this page renders.
			</p>
		</div>
	</section>
</main>

<footer class="border-border/60 border-t">
	<div
		class="text-muted-foreground mx-auto flex max-w-6xl flex-col items-center justify-between gap-3 px-6 py-8 text-sm sm:flex-row"
	>
		<span>deployer-demo · test target for the deployer CLI</span>
		<div class="flex gap-5">
			<a
				class="hover:text-foreground transition-colors"
				href="https://github.com/hra42/deployer"
				target="_blank">deployer</a
			>
			<a class="hover:text-foreground transition-colors" href="https://svelte.dev" target="_blank"
				>SvelteKit</a
			>
			<a
				class="hover:text-foreground transition-colors"
				href="https://traefik.io"
				target="_blank">Traefik</a
			>
		</div>
	</div>
</footer>
