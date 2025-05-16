<script lang="ts">
	import type { OCRPageObject } from '@mistralai/mistralai/models/components';
	import hljs from 'highlight.js/lib/core';
	import { marked } from 'marked';
	import markedKatex from 'marked-katex-extension';
	import CopyIcon from 'lucide-svelte/icons/copy';
	import { getToastStore, type ToastSettings } from '@skeletonlabs/skeleton';

	let { page, loading }: { page: OCRPageObject; loading: boolean } = $props();
	marked.use(markedKatex({ throwOnError: false }));

	const toastStore = getToastStore();

	let showCopyButton = $state(false);

	const rawMarkdownText = $derived.by(() => {
		let text = page.markdown;
		return text;
	});

	const renderedHtmlMarkdown = $derived.by(() => {
		let markdownContent = page.markdown;
		for (let index = 0; index < page.images.length; index++) {
			const image = page.images[index];
			markdownContent = markdownContent.replaceAll(`(${image.id})`, `(${image.imageBase64})`);
		}
		return (marked.parse(markdownContent, { async: false, gfm: true, breaks: true }) as string).trim();
	});

	async function copyToClipboard() {
		try {
			await navigator.clipboard.writeText(rawMarkdownText);
			const toast: ToastSettings = {
				message: 'Copied to clipboard!',
				background: 'variant-filled-success'
			};
			toastStore.trigger(toast);
		} catch (err) {
			console.error('Failed to copy: ', err);
			const toast: ToastSettings = {
				message: 'Failed to copy to clipboard.',
				background: 'variant-filled-error'
			};
			toastStore.trigger(toast);
		}
	}

	$effect(() => {
		page.markdown;
		hljs.highlightAll();
	});
</script>

<div class="text-center">
	<div class="text-neutral-600">{page.index + 1}</div>
</div>
<div
	class="card p-4 variant-ghost-primary text-primary-300 relative"
	role="button"
	tabindex="0"
	onmouseenter={() => (showCopyButton = true)}
	onmouseleave={() => (showCopyButton = false)}
	onfocus={() => (showCopyButton = true)}
	onblur={() => (showCopyButton = false)}
	onkeydown={(e) => {
		if (e.key === 'Enter' || e.key === ' ') {
			copyToClipboard();
		}
	}}
>
	{#if showCopyButton}
		<button
			class="absolute top-2 right-2 btn btn-sm variant-ghost-surface p-1"
			title="Copy OCR text"
			onclick={copyToClipboard}
			tabindex="-1"
		>
			<CopyIcon size={18} />
		</button>
	{/if}
	<div class="space-y-4 rendered-markdown max-w-full overflow-x-hidden">
		{@html renderedHtmlMarkdown}
	</div>
</div>

<style>
</style>
