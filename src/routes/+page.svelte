<script lang="ts">
	import { Card, SectionHeader, Button, Badge, StatusBar, Divider } from '$lib';

	let selectedComponent = $state<string>('card');
	let demoCounter = $state(4);

	const components = [
		{ id: 'card', name: 'Card', description: 'Content container' },
		{ id: 'button', name: 'Button', description: 'Bracket actions' },
		{ id: 'badge', name: 'Badge', description: 'Status indicators' },
		{ id: 'statusbar', name: 'StatusBar', description: 'Footer bar' },
		{ id: 'divider', name: 'Divider', description: 'Section separator' }
	];

	// BentoGridThing-style status bar: SYS key highlighted, others dim
	const statusItems = [
		{ key: 'SYS', value: 'ready', keyHighlight: true },
		{ key: 'COMPONENTS', value: '6', highlight: 'secondary' as const },
		{ key: 'PHASE', value: '1', highlight: 'accent' as const },
		{ key: 'v', value: '0.1.0' }
	];
</script>

<div class="tui-sharp flex min-h-screen flex-col bg-tui-bg">
	<!-- Header -->
	<header class="border-b border-tui-border px-4 py-6">
		<h1 class="crt-glow text-3xl font-bold tracking-tight text-tui-primary">PHOSPHOR</h1>
		<p class="mt-1 font-mono text-sm text-tui-dim">
			<span class="text-tui-secondary">$</span> Terminal-inspired UI components for Svelte 5
		</p>
	</header>

	<!-- Main Content -->
	<main class="flex flex-1 gap-4 p-4">
		<!-- Left Sidebar: Component List -->
		<Card title="COMPONENTS" description="select to preview" class="w-64 shrink-0">
			<nav class="flex flex-col gap-1">
				{#each components as component}
					<Button
						variant="nav"
						active={selectedComponent === component.id}
						onclick={() => (selectedComponent = component.id)}
					>
						<span class="flex w-full items-center gap-2 whitespace-nowrap">
							<span class="shrink-0">{component.name}</span>
							<span class="truncate text-xs font-normal normal-case text-tui-dim">// {component.description}</span>
						</span>
					</Button>
				{/each}
			</nav>
		</Card>

		<!-- Right Panel: Component Preview -->
		<Card title="PREVIEW" description="live component demo" class="flex-1" glow>
			<div class="space-y-6">
				{#if selectedComponent === 'card'}
					<!-- Card Demo -->
					<div class="space-y-3">
						<SectionHeader title="CARD" description="content containers" />
						<div class="grid gap-3 md:grid-cols-2">
							<Card title="BASIC_CARD" description="simple container">
								<p class="text-sm text-tui-dim">
									Cards use the terminal-box pattern with optional title and description.
								</p>
							</Card>
							<Card title="GLOW_CARD" description="with glow effect" glow>
								<p class="text-sm text-tui-dim">
									Add the <code class="text-tui-primary">glow</code> prop for emphasis.
								</p>
							</Card>
						</div>
					</div>
				{:else if selectedComponent === 'button'}
					<!-- Button Demo -->
					<div class="space-y-3">
						<SectionHeader title="BUTTON" description="bracket-style actions" />
						<div class="flex flex-wrap gap-2">
							<Button>Default</Button>
							<Button variant="ghost">Ghost</Button>
							<Button variant="destructive">Delete</Button>
						</div>

						<SectionHeader title="TEXT_VARIANT" description="inline actions" />
						<div class="flex items-center gap-3">
							<span class="text-sm text-tui-dim">counter:</span>
							<Button variant="text" onclick={() => demoCounter--} disabled={demoCounter <= 0}>[-]</Button>
							<span class="w-8 text-center font-bold text-tui-primary">{demoCounter}</span>
							<Button variant="text" onclick={() => demoCounter++} disabled={demoCounter >= 10}>[+]</Button>
						</div>

						<SectionHeader title="SIZES" description="different button sizes" />
						<div class="flex flex-wrap items-center gap-2">
							<Button>Default Size</Button>
							<Button size="sm">Small</Button>
							<Button size="icon">+</Button>
							<Button size="icon">-</Button>
						</div>

						<SectionHeader title="FULL_WIDTH" description="block-level button" />
						<Button full>Full Width Action</Button>

						<SectionHeader title="STATES" description="disabled state" />
						<div class="flex flex-wrap gap-2">
							<Button disabled>Disabled</Button>
							<Button variant="text" disabled>[-]</Button>
						</div>
					</div>
				{:else if selectedComponent === 'badge'}
					<!-- Badge Demo -->
					<div class="space-y-3">
						<SectionHeader title="BADGE" description="status indicators" />
						<div class="flex flex-wrap gap-3">
							<Badge>DEFAULT</Badge>
							<Badge variant="success">ACTIVE</Badge>
							<Badge variant="warning">PENDING</Badge>
							<Badge variant="destructive">ERROR</Badge>
							<Badge variant="info">INFO</Badge>
						</div>
						<SectionHeader title="IN_CONTEXT" description="badges in use" />
						<div class="flex items-center gap-3 border border-tui-border bg-tui-surface p-2">
							<span class="text-sm">System Status:</span>
							<Badge variant="success">ONLINE</Badge>
							<span class="text-tui-border">|</span>
							<span class="text-sm">API:</span>
							<Badge variant="warning">DEGRADED</Badge>
						</div>
					</div>
				{:else if selectedComponent === 'statusbar'}
					<!-- StatusBar Demo -->
					<div class="space-y-3">
						<SectionHeader title="STATUSBAR" description="footer information bar" />
						<div class="overflow-hidden border border-tui-border">
							<div class="bg-tui-surface p-3">
								<p class="text-sm text-tui-dim">
									Keys are dim by default. Use <code class="text-tui-primary">keyHighlight</code> for important keys.
								</p>
							</div>
							<StatusBar
								items={[
									{ key: 'SYS', value: 'ready', keyHighlight: true },
									{ key: 'MODE', value: 'EDIT' },
									{ key: 'LINE', value: '42', highlight: 'secondary' },
									{ key: 'COL', value: '8', highlight: 'accent' }
								]}
							/>
						</div>
						<SectionHeader title="VALUE_COLORS" description="primary, secondary, accent" />
						<div class="overflow-hidden border border-tui-border">
							<div class="bg-tui-surface p-3">
								<p class="text-sm text-tui-dim">
									Values can be highlighted with different colors.
								</p>
							</div>
							<StatusBar
								items={[
									{ key: 'GREEN', value: 'primary', highlight: 'primary' },
									{ key: 'AMBER', value: 'secondary', highlight: 'secondary' },
									{ key: 'CYAN', value: 'accent', highlight: 'accent' },
									{ key: 'PLAIN', value: 'none' }
								]}
							/>
						</div>
					</div>
				{:else if selectedComponent === 'divider'}
					<!-- Divider Demo -->
					<div class="space-y-3">
						<SectionHeader title="DIVIDER" description="section separator" />
						<div class="border border-tui-border bg-tui-surface p-3">
							<p class="text-sm text-tui-dim">Content above the divider</p>
							<Divider class="my-3" />
							<p class="text-sm text-tui-dim">Content below the divider</p>
							<Divider class="my-3" />
							<p class="text-sm text-tui-dim">Another section</p>
						</div>
						<SectionHeader title="IN_CARD" description="separating card sections" />
						<Card title="SETTINGS" description="example card">
							<div class="space-y-3">
								<div class="flex items-center justify-between">
									<span class="text-sm">Option One</span>
									<Badge variant="success">ON</Badge>
								</div>
								<Divider />
								<div class="flex items-center justify-between">
									<span class="text-sm">Option Two</span>
									<Badge>OFF</Badge>
								</div>
								<Divider />
								<div class="flex items-center justify-between">
									<span class="text-sm">Option Three</span>
									<Badge variant="warning">BETA</Badge>
								</div>
							</div>
						</Card>
					</div>
				{/if}
			</div>
		</Card>
	</main>

	<!-- Footer Status Bar -->
	<StatusBar items={statusItems} />
</div>
