<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({ transition: 'slide' })
</script>

<script>
	import { onMount } from 'svelte'
	import { tween } from '@animotion/motion'

	let boxScale = tween(0, { duration: 400 })
	let boxRotate = tween(-20, { duration: 600 })
	let trailOpacity = tween(0, { duration: 300 })
	let textOpacity = tween(0, { duration: 500 })
	let slideEl

	async function playAnimation() {
		boxScale.reset(); boxRotate.reset(); trailOpacity.reset(); textOpacity.reset()
		await boxScale.to(1)
		await trailOpacity.to(1)
		await boxRotate.to(0)
		await textOpacity.to(1)
	}

	onMount(() => {
		const section = slideEl.closest('section')
		section.addEventListener('in', playAnimation)
		playAnimation()
		return () => section.removeEventListener('in', playAnimation)
	})
</script>

<div class="divider-slide" bind:this={slideEl}>
	<div style="position: relative; width: 120px; height: 120px; display: flex; align-items: center; justify-content: center;">
		<!-- Motion trail ghosts -->
		{#each [0.12, 0.22, 0.35] as opacity, i}
			<div
				style="position: absolute; width: 56px; height: 56px; border-radius: 14px; background: linear-gradient(135deg, var(--purple), var(--blue)); opacity: {trailOpacity.current * opacity}; transform: scale({boxScale.current * (0.7 + i * 0.1)}) rotate({boxRotate.current - (3 - i) * 8}deg) translate({(2 - i) * -6}px, {(2 - i) * 4}px); filter: blur({(3 - i) * 2}px);"
			></div>
		{/each}
		<!-- Main box -->
		<div
			style="width: 56px; height: 56px; border-radius: 14px; background: linear-gradient(135deg, var(--purple), var(--blue)); box-shadow: 0 0 30px rgba(168, 139, 250, 0.4); transform: scale({boxScale.current}) rotate({boxRotate.current}deg);"
		></div>
	</div>

	<p style="font-size: 1.2rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.15em; font-weight: 600; margin-top: 2rem; opacity: {textOpacity.current};">
		Part 2
	</p>
	<p class="gradient-text" style="font-size: 3.5rem; font-weight: 800; letter-spacing: -0.03em; margin-top: 0.5rem; opacity: {textOpacity.current};">
		Motion
	</p>
	<p style="font-size: 1.3rem; color: var(--text-muted); margin-top: 0.8rem; opacity: {textOpacity.current};">
		Batteries included
	</p>
</div>

<aside class="notes">
Transition to Part 2. Now let us see what Motion adds on top of WAAPI.
Fun fact: Motion actually uses WAAPI under the hood for transforms and opacity.
</aside>
