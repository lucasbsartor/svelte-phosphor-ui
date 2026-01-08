<script lang="ts">
	type HighlightColor = 'primary' | 'secondary' | 'accent';

	interface StatusItem {
		key: string;
		value: string;
		highlight?: boolean | HighlightColor;
		keyHighlight?: boolean; // Makes the key green instead of dim
	}

	interface Props {
		items: StatusItem[];
		class?: string;
	}

	let { items, class: className = '' }: Props = $props();

	function getValueClass(highlight: boolean | HighlightColor | undefined): string {
		if (!highlight) return 'text-tui-text';
		if (highlight === true || highlight === 'primary') return 'text-tui-primary';
		if (highlight === 'secondary') return 'text-tui-secondary';
		if (highlight === 'accent') return 'text-tui-accent';
		return 'text-tui-text';
	}
</script>

<footer
	class="flex items-center gap-3 border-t border-tui-border bg-tui-surface px-3 py-1.5 font-mono text-xs {className}"
>
	{#each items as item, i}
		<div class="flex items-center gap-1">
			<span class={item.keyHighlight ? 'text-tui-primary' : 'text-tui-dim'}>{item.key}:</span>
			<span class={getValueClass(item.highlight)}>
				{item.value}
			</span>
		</div>
		{#if i < items.length - 1}
			<span class="text-tui-border">|</span>
		{/if}
	{/each}
</footer>
