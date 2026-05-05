<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'

	let code: ReturnType<typeof Code>
	let bars: HTMLDivElement[] = []

	function runStagger() {
		requestAnimationFrame(() => {
			bars.forEach((bar, i) => {
				if (!bar) return
				bar.getAnimations().forEach((a) => a.cancel())
				bar.style.transform = 'scaleX(0)'
				bar.style.opacity = '0'
				bar.animate(
					[
						{ transform: 'scaleX(0)', opacity: 0 },
						{ transform: 'scaleX(1)', opacity: 1 }
					],
					{
						duration: 400,
						delay: i * 100,
						easing: 'ease-out',
						fill: 'forwards'
					}
				)
			})
		})
	}

	function resetStagger() {
		bars.forEach((bar) => {
			if (!bar) return
			bar.getAnimations().forEach((a) => a.cancel())
			bar.style.transform = 'scaleX(0)'
			bar.style.opacity = '0'
		})
	}

	const colors = ['var(--amber)', 'var(--cyan)', 'var(--purple)', 'var(--emerald)', 'var(--pink)']
	const widths = ['85%', '70%', '95%', '60%', '78%']
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Staggering
		</p>
	</Transition>

	<Transition do={runStagger} undo={resetStagger}>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 2800px;">
			<div style="flex: 1; min-width: 700px; overflow: hidden;">
				<Code
					bind:this={code}
					lang="js"
					theme="poimandres"
					code={`const bars = document.querySelectorAll('.bar')

bars.forEach((bar, i) => {
  bar.animate([
    { transform: 'scaleX(0)', opacity: 0 },
    { transform: 'scaleX(1)', opacity: 1 }
  ], {
    duration: 400,
    delay: i * 100,
    easing: 'ease-out',
    fill: 'forwards'
  })
})`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 1; min-width: 300px;">
				<div class="demo-area" style="flex-direction: column; gap: 0.6rem; padding: 1.5rem; min-height: 220px;">
					{#each colors as color, i}
						<div
							bind:this={bars[i]}
							style="height: 22px; width: {widths[i]}; background: {color}; border-radius: 6px; transform-origin: left; transform: scaleX(0); opacity: 0;"
						></div>
					{/each}
				</div>
			</div>
		</div>
	</Transition>

	<Action
		do={() => code.selectLines`9`}
		undo={() => code.selectLines`*`}
	/>

	<Action
		do={async () => {
			resetStagger()
			runStagger()
			await code.selectLines`*`
		}}
		undo={() => code.selectLines`9`}
	/>
</div>
