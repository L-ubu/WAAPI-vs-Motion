<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'

	let code: ReturnType<typeof Code>
	let demoBox: HTMLDivElement

	function runAnimation() {
		if (!demoBox) return
		demoBox.getAnimations().forEach((a) => a.cancel())
		demoBox.style.opacity = '0'
		demoBox.style.transform = 'translateX(-60px)'
		demoBox.animate(
			[
				{ opacity: 0, transform: 'translateX(-60px)' },
				{ opacity: 1, transform: 'translateX(0)' }
			],
			{ duration: 600, easing: 'ease-out', fill: 'forwards' }
		)
	}

	function resetDemo() {
		if (!demoBox) return
		demoBox.getAnimations().forEach((a) => a.cancel())
		demoBox.style.opacity = '0'
		demoBox.style.transform = 'translateX(-60px)'
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			<code style="color: var(--amber); background: var(--surface); padding: 4px 14px; border-radius: 8px; font-size: 1.6rem;">element.animate()</code>
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="js"
					theme="poimandres"
					code={`const box = document.querySelector('.box')

box.animate(
  [
    { opacity: 0, transform: 'translateX(-60px)' },
    { opacity: 1, transform: 'translateX(0)' }
  ],
  {
    duration: 600,
    easing: 'ease-out',
    fill: 'forwards'
  }
)`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="min-height: 220px;">
					<div
						bind:this={demoBox}
						style="width: 90px; height: 90px; border-radius: 16px; background: linear-gradient(135deg, var(--amber), #f59e0b); opacity: 0; transform: translateX(-60px); box-shadow: 0 8px 30px rgba(251, 191, 36, 0.3);"
					></div>
				</div>
			</div>
		</div>
	</Transition>

	<Action do={runAnimation} undo={resetDemo} />

	<Action
		do={async () => {
			resetDemo()
			await code.selectLines`9-12`
		}}
		undo={async () => {
			await code.selectLines`*`
		}}
	/>

	<Action
		do={async () => {
			runAnimation()
			await code.selectLines`*`
		}}
		undo={async () => {
			await code.selectLines`9-12`
		}}
	/>
</div>

<aside class="notes">
This is the core API - element.animate().
Takes keyframes array and options object. Returns an Animation object you can control.
Demo: click to see the box animate.
</aside>
