<script lang="ts">
	import type { Snippet } from 'svelte';
	import type { HTMLButtonAttributes } from 'svelte/elements';

	type Variant = 'default' | 'ghost' | 'destructive' | 'text' | 'nav';
	type Size = 'default' | 'sm' | 'icon';

	interface Props extends HTMLButtonAttributes {
		variant?: Variant;
		size?: Size;
		full?: boolean;
		active?: boolean;
		children?: Snippet;
	}

	let {
		variant = 'default',
		size = 'default',
		full = false,
		active = false,
		class: className = '',
		disabled = false,
		children,
		...restProps
	}: Props = $props();

	// Text and nav variants don't use brackets
	let isTextVariant = $derived(variant === 'text');
	let isNavVariant = $derived(variant === 'nav');

	const baseStyles = `
		inline-flex items-center justify-center
		font-mono font-medium uppercase tracking-wide
		transition-colors duration-75
		focus:outline-none focus-visible:glow-border
		disabled:opacity-50 disabled:cursor-not-allowed
	`;

	const variantStyles: Record<Variant, string> = {
		default: `
			border border-tui-border bg-tui-surface text-tui-text
			hover:border-tui-primary hover:text-tui-primary hover:crt-glow
			active:bg-tui-primary/10
		`,
		ghost: `
			border border-transparent bg-transparent text-tui-dim
			hover:text-tui-text hover:border-tui-border
			active:bg-tui-surface
		`,
		destructive: `
			border border-tui-destructive/50 bg-tui-surface text-tui-destructive
			hover:border-tui-destructive hover:bg-tui-destructive/10
			active:bg-tui-destructive/20
		`,
		text: `
			border-none bg-transparent text-tui-primary
			hover:text-tui-secondary
			disabled:text-tui-border
		`,
		nav: `
			border bg-tui-surface text-tui-text
			hover:border-tui-primary hover:text-tui-primary
		`
	};

	// Active state styles for nav variant
	let navActiveStyles = $derived(
		active
			? 'border-tui-primary bg-tui-primary/10 text-tui-primary'
			: 'border-tui-border'
	);

	const sizeStyles: Record<Size, string> = {
		default: 'h-8 px-3 text-sm',
		sm: 'h-6 px-2 text-xs',
		icon: 'h-8 w-8 text-sm'
	};

	// Text variant uses minimal sizing
	const textSizeStyles: Record<Size, string> = {
		default: 'text-sm',
		sm: 'text-xs',
		icon: 'text-sm'
	};

	// Nav variant uses full-width sizing with left alignment
	const navSizeStyles: Record<Size, string> = {
		default: 'w-full px-2 py-1.5 text-sm text-left justify-start',
		sm: 'w-full px-2 py-1 text-xs text-left justify-start',
		icon: 'w-full px-2 py-1.5 text-sm text-left justify-start'
	};

	let computedSizeStyles = $derived(
		isTextVariant ? textSizeStyles[size] : isNavVariant ? navSizeStyles[size] : sizeStyles[size]
	);
</script>

<button
	class="{baseStyles} {variantStyles[variant]} {isNavVariant ? navActiveStyles : ''} {computedSizeStyles} {full ? 'w-full' : ''} {className}"
	{disabled}
	{...restProps}
>
	{#if isTextVariant}
		{@render children?.()}
	{:else if isNavVariant}
		<span class="flex w-full items-center gap-1.5">
			<span class={active ? 'text-tui-primary' : 'text-tui-dim'}>{active ? '>' : '\u00A0'}</span>
			{@render children?.()}
		</span>
	{:else}
		<span class="flex items-center gap-0.5">
			<span class="text-tui-dim">[</span>
			{@render children?.()}
			<span class="text-tui-dim">]</span>
		</span>
	{/if}
</button>
