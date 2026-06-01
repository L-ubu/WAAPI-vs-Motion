<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'

	let code: ReturnType<typeof Code>
	let progressBar: HTMLDivElement
	let scrollContainer: HTMLDivElement

	function setupScrollDemo() {
		if (!progressBar || !scrollContainer) return
		const anim = progressBar.animate(
			[{ width: '0%' }, { width: '100%' }],
			{
				duration: 1,
				fill: 'forwards',
				timeline: new ScrollTimeline({
					source: scrollContainer,
					axis: 'block'
				})
			}
		)
		return anim
	}

	function resetDemo() {
		if (!progressBar) return
		progressBar.getAnimations().forEach((a) => a.cancel())
		progressBar.style.width = '0%'
		if (scrollContainer) scrollContainer.scrollTop = 0
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Scroll-Driven Animations
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="js"
					theme="poimandres"
					code={`bar.animate(
  [{ width: '0%' }, { width: '100%' }],
  {
    fill: 'forwards',
    timeline: new ScrollTimeline({
      source: scrollContainer,
      axis: 'block'
    })
  }
)`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 1rem; min-height: 220px;">
				<div style="width: 100%; max-width: 200px; height: 8px; background: var(--surface); border-radius: 4px; overflow: hidden; border: 1px solid var(--border);">
					<div bind:this={progressBar} style="height: 100%; width: 0%; background: var(--amber); border-radius: 4px; transition: none;"></div>
				</div>
				<div
					bind:this={scrollContainer}
					style="width: 100%; max-width: 200px; height: 140px; overflow-y: auto; background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1rem; font-size: 0.75rem; color: var(--text-muted); line-height: 1.8;"
				>
					<p>Scroll me down...</p>
					<p style="margin-top: 0.5rem;">The progress bar above tracks scroll position using pure WAAPI.</p>
					<p style="margin-top: 0.5rem;">No JavaScript scroll listeners needed.</p>
					<p style="margin-top: 0.5rem;">No requestAnimationFrame loops.</p>
					<p style="margin-top: 0.5rem;">Runs on the compositor thread - buttery smooth 60fps.</p>
					<p style="margin-top: 0.5rem;">This is the future of scroll animations.</p>
					<p style="margin-top: 0.5rem;">All native. All performant.</p>
					<p style="margin-top: 1rem; color: var(--amber);">You've reached the end!</p>
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted);">Try scrolling the box above</p>
			</div>
		</div>
	</Transition>

	<Action do={setupScrollDemo} undo={resetDemo} />

	<Action
		do={() => code.selectLines`5-8`}
		undo={() => code.selectLines`*`}
	/>

	<Transition>
		<p style="font-size: 0.85rem; color: var(--text-muted); margin-top: 0.5rem;">
			<span style="color: var(--amber);">Note:</span> ScrollTimeline has ~85% browser support (Chrome, Edge, Firefox)
		</p>
	</Transition>
</div>

<aside class="notes">
Scroll-driven animations run entirely on compositor thread - 60fps guaranteed, no JS on scroll.
Chrome 115+, Firefox 110+. Safari working on it.
This is a game-changer for scroll effects.
</aside>
