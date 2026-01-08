<script lang="ts">
	interface Props {
		title?: string;
		description?: string;
		glow?: boolean;
		class?: string;
		children?: import('svelte').Snippet;
	}

	let { title, description, glow = false, class: className = '', children }: Props = $props();
</script>

<div class="terminal-box p-3 {glow ? 'glow-border' : ''} {className}">
	{#if title}
		<header class="mb-2 flex items-baseline gap-1.5">
			<span class="text-tui-primary">&gt;</span>
			<h3 class="text-sm font-bold uppercase tracking-wide text-tui-secondary">
				{title}
			</h3>
			{#if description}
				<span class="text-xs text-tui-dim">// {description}</span>
			{/if}
		</header>
	{/if}
	<div class="card-content">
		{#if children}
			{@render children()}
		{/if}
	</div>
</div>
