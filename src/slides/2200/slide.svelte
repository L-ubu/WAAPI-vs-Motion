<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let waapiBars: HTMLDivElement[] = []
	let motionBars: HTMLDivElement[] = []

	const barColors = ['var(--amber)', 'var(--cyan)', 'var(--emerald)', 'var(--pink)', 'var(--purple)']
	const barWidths = ['80%', '65%', '90%', '55%', '72%']

	function staggerWaapi() {
		waapiBars.forEach((bar, i) => {
			if (!bar) return
			bar.getAnimations().forEach((a) => a.cancel())
			bar.style.transform = 'scaleX(0)'
			bar.style.opacity = '0'
			bar.animate(
				[{ transform: 'scaleX(0)', opacity: 0 }, { transform: 'scaleX(1)', opacity: 1 }],
				{ duration: 400, delay: i * 100, easing: 'ease-out', fill: 'forwards' }
			)
		})
	}

	function staggerMotion() {
		motionBars.forEach((bar, i) => {
			if (!bar) return
			bar.getAnimations().forEach((a) => a.cancel())
			animate(bar, { scaleX: [0, 1], opacity: [0, 1] }, { duration: 0.4, delay: i * 0.1, easing: 'ease-out' })
		})
	}

	function resetAll() {
		;[...waapiBars, ...motionBars].forEach((bar) => {
			if (!bar) return
			bar.getAnimations().forEach((a) => a.cancel())
			bar.style.transform = 'scaleX(0)'
			bar.style.opacity = '0'
		})
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Stagger - verbose vs declarative
		</p>
	</Transition>

	<div style="display: flex; gap: 3rem; align-items: stretch; width: 100%; max-width: 800px;">
		<div class="panel">
			<div class="panel-header" style="color: var(--amber);">WAAPI</div>
			<div class="panel-body" style="flex-direction: column; gap: 1.2rem;">
				<code style="font-size: 0.6rem; color: var(--text-muted); white-space: pre; line-height: 1.4; font-family: var(--r-code-font); background: rgba(255,255,255,0.03); padding: 0.5rem 0.8rem; border-radius: 8px; border: 1px solid var(--border); align-self: stretch;">{'bars.forEach((bar, i) =>\n  bar.animate(keyframes, {\n    delay: i * 100\n  })\n)'}</code>
				<div style="display: flex; flex-direction: column; gap: 0.35rem; width: 100%;">
					{#each barColors as color, i}
						<div
							bind:this={waapiBars[i]}
							style="height: 16px; width: {barWidths[i]}; background: {color}; border-radius: 4px; transform-origin: left; transform: scaleX(0); opacity: 0;"
						></div>
					{/each}
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted);">8 lines of JS</p>
			</div>
		</div>

		<div class="panel">
			<div class="panel-header" style="color: var(--purple);">Motion</div>
			<div class="panel-body" style="flex-direction: column; gap: 1.2rem;">
				<code style="font-size: 0.6rem; color: var(--text-muted); white-space: pre; line-height: 1.4; font-family: var(--r-code-font); background: rgba(255,255,255,0.03); padding: 0.5rem 0.8rem; border-radius: 8px; border: 1px solid var(--border); align-self: stretch;">staggerChildren: 0.1</code>
				<div style="display: flex; flex-direction: column; gap: 0.35rem; width: 100%;">
					{#each barColors as color, i}
						<div
							bind:this={motionBars[i]}
							style="height: 16px; width: {barWidths[i]}; background: {color}; border-radius: 4px; transform-origin: left; transform: scaleX(0); opacity: 0;"
						></div>
					{/each}
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted);">1 line of config</p>
			</div>
		</div>
	</div>

	<Action
		do={() => { staggerWaapi(); staggerMotion() }}
		undo={resetAll}
	/>

	<Action
		do={() => { resetAll(); setTimeout(() => { staggerWaapi(); staggerMotion() }, 200) }}
		undo={resetAll}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			Same output - dramatically different <span style="color: var(--purple); font-weight: 600;">developer experience</span>
		</p>
	</Transition>
</div>

<aside class="notes">
WAAPI: forEach loop, manual delay math.
Motion: staggerChildren in parent variant, done.
Same visual result but much less code and mental overhead.
</aside>
