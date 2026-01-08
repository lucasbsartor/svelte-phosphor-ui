<script lang="ts">
	interface Props {
		title: string;
		description?: string;
		class?: string;
	}

	let { title, description, class: className = '' }: Props = $props();
</script>

<header class="flex items-baseline gap-1.5 {className}">
	<span class="text-tui-primary">&gt;</span>
	<h3 class="text-sm font-bold uppercase tracking-wide text-tui-secondary">
		{title}
	</h3>
	{#if description}
		<span class="text-xs text-tui-dim">// {description}</span>
	{/if}
</header>
