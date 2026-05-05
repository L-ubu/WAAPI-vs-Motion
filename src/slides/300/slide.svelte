<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({ transition: 'slide' })
</script>

<script>
	import { onMount } from 'svelte'
	import { tween } from '@animotion/motion'

	let frameStroke = tween(0, { duration: 600 })
	let dotsScale = tween(0, { duration: 300 })
	let bar1 = tween(0, { duration: 300 })
	let bar2 = tween(0, { duration: 300 })
	let bar3 = tween(0, { duration: 300 })
	let bar4 = tween(0, { duration: 300 })
	let pulseOpacity = tween(0, { duration: 400 })
	let textOpacity = tween(0, { duration: 500 })
	let slideEl

	async function playAnimation() {
		frameStroke.reset(); dotsScale.reset(); bar1.reset(); bar2.reset(); bar3.reset(); bar4.reset(); pulseOpacity.reset(); textOpacity.reset()
		await frameStroke.to(1)
		await dotsScale.to(1)
		await bar1.to(1)
		await bar2.to(1)
		await bar3.to(1)
		await bar4.to(1)
		await pulseOpacity.to(1)
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
	<svg width="140" height="110" viewBox="0 0 140 110" style="overflow: visible;">
		<!-- Browser frame -->
		<rect
			x="10" y="10" width="120" height="90" rx="10"
			fill="none" stroke="var(--amber)" stroke-width="2.5"
			pathLength="1" stroke-dasharray="1"
			style="stroke-dashoffset: {1 - frameStroke.current};"
		/>
		<!-- Title bar -->
		<line
			x1="10" y1="32" x2="130" y2="32"
			stroke="var(--amber)" stroke-width="1.5"
			style="opacity: {frameStroke.current * 0.4};"
		/>
		<!-- Traffic light dots -->
		<circle cx="25" cy="21" r="3.5" fill="#ff5f57" style="transform: scale({dotsScale.current}); transform-origin: 25px 21px;" />
		<circle cx="37" cy="21" r="3.5" fill="#febc2e" style="transform: scale({dotsScale.current}); transform-origin: 37px 21px;" />
		<circle cx="49" cy="21" r="3.5" fill="#28c840" style="transform: scale({dotsScale.current}); transform-origin: 49px 21px;" />
		<!-- Code lines appearing one by one -->
		<rect x="25" y="44" width="90" height="4" rx="2" fill="var(--amber)"
			style="opacity: {bar1.current * 0.5}; transform: scaleX({bar1.current}); transform-origin: 25px 46px;" />
		<rect x="25" y="56" width="65" height="4" rx="2" fill="var(--amber)"
			style="opacity: {bar2.current * 0.4}; transform: scaleX({bar2.current}); transform-origin: 25px 58px;" />
		<rect x="25" y="68" width="78" height="4" rx="2" fill="var(--amber)"
			style="opacity: {bar3.current * 0.3}; transform: scaleX({bar3.current}); transform-origin: 25px 70px;" />
		<rect x="25" y="80" width="50" height="4" rx="2" fill="var(--amber)"
			style="opacity: {bar4.current * 0.25}; transform: scaleX({bar4.current}); transform-origin: 25px 82px;" />
		<!-- Pulse ring -->
		<circle cx="70" cy="65" r="45" fill="none" stroke="var(--amber)" stroke-width="1"
			style="opacity: {pulseOpacity.current * 0.15};">
			<animate attributeName="r" values="45;55;45" dur="2s" repeatCount="indefinite" />
			<animate attributeName="opacity" values="0.15;0.05;0.15" dur="2s" repeatCount="indefinite" />
		</circle>
	</svg>

	<p style="font-size: 1.2rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.15em; font-weight: 600; margin-top: 1.5rem; opacity: {textOpacity.current};">
		Part 1
	</p>
	<p class="gradient-text-amber" style="font-size: 3.5rem; font-weight: 800; letter-spacing: -0.03em; margin-top: 0.5rem; opacity: {textOpacity.current};">
		Web Animations API
	</p>
	<p style="font-size: 1.3rem; color: var(--text-muted); margin-top: 0.8rem; opacity: {textOpacity.current};">
		The native animation primitive
	</p>
</div>
