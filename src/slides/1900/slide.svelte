<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({ transition: 'slide' })
</script>

<script>
	import { onMount } from 'svelte'
	import { tween } from '@animotion/motion'

	let leftSlide = tween(0, { duration: 500 })
	let rightSlide = tween(0, { duration: 500 })
	let leftStroke = tween(0, { duration: 400 })
	let rightStroke = tween(0, { duration: 400 })
	let labelOpacity = tween(0, { duration: 300 })
	let vsScale = tween(0, { duration: 400 })
	let connectLine = tween(0, { duration: 300 })
	let textOpacity = tween(0, { duration: 500 })
	let slideEl

	async function playAnimation() {
		leftSlide.reset(); rightSlide.reset(); leftStroke.reset(); rightStroke.reset(); labelOpacity.reset(); vsScale.reset(); connectLine.reset(); textOpacity.reset()
		await Promise.all([leftSlide.to(1), rightSlide.to(1)])
		await Promise.all([leftStroke.to(1), rightStroke.to(1)])
		await labelOpacity.to(1)
		await vsScale.to(1)
		await connectLine.to(1)
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
	<svg width="200" height="100" viewBox="0 0 200 100" style="overflow: visible;">
		<!-- Left panel - WAAPI -->
		<g style="transform: translateX({(1 - leftSlide.current) * -40}px); opacity: {leftSlide.current};">
			<rect
				x="10" y="10" width="70" height="80" rx="10"
				fill="none" stroke="var(--amber)" stroke-width="2.5"
				pathLength="1" stroke-dasharray="1"
				style="stroke-dashoffset: {1 - leftStroke.current};"
			/>
			<!-- Mini code lines inside -->
			<rect x="22" y="30" width="46" height="3" rx="1.5" fill="var(--amber)" style="opacity: {labelOpacity.current * 0.3};" />
			<rect x="22" y="38" width="32" height="3" rx="1.5" fill="var(--amber)" style="opacity: {labelOpacity.current * 0.25};" />
			<rect x="22" y="46" width="40" height="3" rx="1.5" fill="var(--amber)" style="opacity: {labelOpacity.current * 0.2};" />
			<text x="45" y="70" text-anchor="middle" fill="var(--amber)" font-size="11" font-family="var(--r-code-font)" font-weight="700"
				style="opacity: {labelOpacity.current};">WAAPI</text>
		</g>

		<!-- Right panel - Motion -->
		<g style="transform: translateX({(1 - rightSlide.current) * 40}px); opacity: {rightSlide.current};">
			<rect
				x="120" y="10" width="70" height="80" rx="10"
				fill="none" stroke="var(--purple)" stroke-width="2.5"
				pathLength="1" stroke-dasharray="1"
				style="stroke-dashoffset: {1 - rightStroke.current};"
			/>
			<!-- Mini spring inside -->
			<path d="M140 30 Q135 38, 145 42 Q135 46, 145 52 Q135 56, 140 60"
				fill="none" stroke="var(--purple)" stroke-width="1.5" stroke-linecap="round"
				style="opacity: {labelOpacity.current * 0.3};" />
			<text x="155" y="70" text-anchor="middle" fill="var(--purple)" font-size="11" font-family="var(--r-code-font)" font-weight="700"
				style="opacity: {labelOpacity.current};">Motion</text>
		</g>

		<!-- VS badge -->
		<g style="transform: scale({vsScale.current}); transform-origin: 100px 50px;">
			<circle cx="100" cy="50" r="14" fill="var(--surface)" stroke="var(--border)" stroke-width="1.5" />
			<text x="100" y="55" text-anchor="middle" fill="var(--text-muted)" font-size="10" font-weight="700" font-family="var(--r-code-font)">vs</text>
		</g>

		<!-- Connecting dashed lines -->
		<line x1="82" y1="50" x2="86" y2="50" stroke="var(--border)" stroke-width="1.5" stroke-dasharray="3 2"
			style="opacity: {connectLine.current * 0.5};" />
		<line x1="114" y1="50" x2="118" y2="50" stroke="var(--border)" stroke-width="1.5" stroke-dasharray="3 2"
			style="opacity: {connectLine.current * 0.5};" />
	</svg>

	<p style="font-size: 1.2rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.15em; font-weight: 600; margin-top: 1.5rem; opacity: {textOpacity.current};">
		Part 3
	</p>
	<p class="gradient-text" style="font-size: 3.5rem; font-weight: 800; letter-spacing: -0.03em; margin-top: 0.5rem; opacity: {textOpacity.current};">
		Side by Side
	</p>
	<p style="font-size: 1.3rem; color: var(--text-muted); margin-top: 0.8rem; opacity: {textOpacity.current};">
		Same animation, different approaches
	</p>
</div>

<aside class="notes">
Motion is not free - adds bundle size (around 16kb).
Requires React or specific framework binding.
Can be overkill for simple fades. WAAPI is lighter when you do not need Motion features.
</aside>
