<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { onMount } from 'svelte'
	import { tween } from '@animotion/motion'

	let textOpacity = tween(0, { duration: 800 })
	let subOpacity = tween(0, { duration: 600 })
	let slideEl

	async function playAnimation() {
		textOpacity.reset(); subOpacity.reset()
		await textOpacity.to(1)
		await subOpacity.to(1)
	}

	onMount(() => {
		const section = slideEl.closest('section')
		section.addEventListener('in', playAnimation)
		playAnimation()
		return () => section.removeEventListener('in', playAnimation)
	})
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;" bind:this={slideEl}>
	<p class="gradient-text" style="font-size: 2.8rem; font-weight: 800; letter-spacing: -0.03em; text-align: center; max-width: 750px; line-height: 1.3; opacity: {textOpacity.current};">
		Start with WAAPI.<br>Reach for Motion when you need more.
	</p>

	<p style="font-size: 1.2rem; color: var(--text-muted); opacity: {subOpacity.current}; text-align: center; max-width: 550px;">
		They're complementary - not competing.<br>Motion is built on top of WAAPI.
	</p>
</div>

<aside class="notes">
Not a competition - they are complementary.
Use WAAPI when you can, reach for Motion when you need its features.
In our codebase most animations can be WAAPI. Reach for Motion for gesture-heavy or layout-animated components.
</aside>
