<script lang="ts">
	import '../styles/app.css'
	import type { Snippet } from 'svelte'

	let { children }: { children: Snippet } = $props()

	function handleKeydown(e: KeyboardEvent) {
		if (e.key === 'm' || e.key === 'M') {
			const root = document.documentElement
			if (root.dataset.theme === 'light') {
				delete root.dataset.theme
			} else {
				root.dataset.theme = 'light'
			}
		}
	}
</script>

<svelte:window onkeydown={handleKeydown} />

<svelte:head>
	<title>Web Animations: WAAPI vs Motion</title>
</svelte:head>

{@render children()}
