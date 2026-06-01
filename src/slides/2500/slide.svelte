<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({ transition: 'slide' })
</script>

<script>
	import { onMount } from 'svelte'
	import { tween } from '@animotion/motion'

	let poleStroke = tween(0, { duration: 500 })
	let baseStroke = tween(0, { duration: 400 })
	let beamStroke = tween(0, { duration: 500 })
	let beamRotation = tween(-12, { duration: 400 })
	let leftPan = tween(0, { duration: 300 })
	let rightPan = tween(0, { duration: 300 })
	let labelOpacity = tween(0, { duration: 300 })
	let balanceSettle = tween(0, { duration: 500 })
	let textOpacity = tween(0, { duration: 500 })
	let slideEl

	async function playAnimation() {
		poleStroke.reset(); baseStroke.reset(); beamStroke.reset(); beamRotation.reset(); leftPan.reset(); rightPan.reset(); labelOpacity.reset(); balanceSettle.reset(); textOpacity.reset()
		await poleStroke.to(1)
		await baseStroke.to(1)
		await beamStroke.to(1)
		await Promise.all([leftPan.to(1), rightPan.to(1)])
		await labelOpacity.to(1)
		await beamRotation.to(5)
		await balanceSettle.to(1)
		await textOpacity.to(1)
	}

	$effect(() => {
		rotation = beamRotation.current * (1 - balanceSettle.current)
	})

	let rotation = $state(-12)

	onMount(() => {
		const section = slideEl.closest('section')
		section.addEventListener('in', playAnimation)
		playAnimation()
		return () => section.removeEventListener('in', playAnimation)
	})
</script>

<div class="divider-slide" bind:this={slideEl}>
	<svg width="160" height="120" viewBox="0 0 160 120" style="overflow: visible;">
		<!-- Center pole -->
		<line x1="80" y1="20" x2="80" y2="85"
			stroke="var(--emerald)" stroke-width="3" stroke-linecap="round"
			pathLength="1" stroke-dasharray="1"
			style="stroke-dashoffset: {1 - poleStroke.current};" />

		<!-- Base -->
		<path d="M55 85 L105 85" fill="none" stroke="var(--emerald)" stroke-width="3" stroke-linecap="round"
			pathLength="1" stroke-dasharray="1"
			style="stroke-dashoffset: {1 - baseStroke.current};" />

		<!-- Fulcrum triangle -->
		<circle cx="80" cy="20" r="5" fill="var(--emerald)"
			style="opacity: {poleStroke.current}; transform: scale({poleStroke.current}); transform-origin: 80px 20px;" />

		<!-- Beam with rotation -->
		<g style="transform: rotate({rotation}deg); transform-origin: 80px 28px;">
			<line x1="25" y1="28" x2="135" y2="28"
				stroke="var(--emerald)" stroke-width="2.5" stroke-linecap="round"
				pathLength="1" stroke-dasharray="1"
				style="stroke-dashoffset: {1 - beamStroke.current};" />

			<!-- Left pan - WAAPI -->
			<g style="opacity: {leftPan.current};">
				<line x1="25" y1="28" x2="25" y2="45" stroke="var(--amber)" stroke-width="1.5" />
				<rect x="10" y="45" width="30" height="20" rx="5" fill="none" stroke="var(--amber)" stroke-width="1.5" />
				<text x="25" y="59" text-anchor="middle" fill="var(--amber)" font-size="7" font-family="var(--r-code-font)"
					style="opacity: {labelOpacity.current};">W</text>
			</g>

			<!-- Right pan - Motion -->
			<g style="opacity: {rightPan.current};">
				<line x1="135" y1="28" x2="135" y2="45" stroke="var(--purple)" stroke-width="1.5" />
				<rect x="120" y="45" width="30" height="20" rx="5" fill="none" stroke="var(--purple)" stroke-width="1.5" />
				<text x="135" y="59" text-anchor="middle" fill="var(--purple)" font-size="7" font-family="var(--r-code-font)"
					style="opacity: {labelOpacity.current};">M</text>
			</g>
		</g>
	</svg>

	<p style="font-size: 1.2rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.15em; font-weight: 600; margin-top: 1.5rem; opacity: {textOpacity.current};">
		Part 4
	</p>
	<p class="gradient-text-emerald" style="font-size: 3.5rem; font-weight: 800; letter-spacing: -0.03em; margin-top: 0.5rem; opacity: {textOpacity.current};">
		Verdict
	</p>
	<p style="font-size: 1.3rem; color: var(--text-muted); margin-top: 0.8rem; opacity: {textOpacity.current};">
		Complementary, not competing
	</p>
</div>

<aside class="notes">
WAAPI: 0kb - it is in the browser already.
Motion: around 16kb gzipped. For micro-frontends or perf-critical apps this matters.
For most apps 16kb is negligible.
</aside>
