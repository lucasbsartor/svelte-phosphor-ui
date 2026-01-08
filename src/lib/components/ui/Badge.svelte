<script lang="ts">
	import type { Snippet } from 'svelte';

	type Variant = 'default' | 'success' | 'warning' | 'destructive' | 'info';

	interface Props {
		variant?: Variant;
		class?: string;
		children?: Snippet;
	}

	let { variant = 'default', class: className = '', children }: Props = $props();

	const baseStyles = `
		inline-flex items-center
		font-mono text-xs font-medium uppercase tracking-wider
	`;

	const variantStyles: Record<Variant, string> = {
		default: 'text-tui-dim',
		success: 'text-tui-primary',
		warning: 'text-tui-secondary',
		destructive: 'text-tui-destructive',
		info: 'text-tui-accent'
	};
</script>

<span class="{baseStyles} {variantStyles[variant]} {className}">
	<span class="text-tui-dim">[</span>
	{#if children}
		{@render children()}
	{/if}
	<span class="text-tui-dim">]</span>
</span>
