<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let code: ReturnType<typeof Code>
	let demoBox: HTMLDivElement
	let currentAnim: any = null

	function runMotionDemo() {
		if (!demoBox) return
		currentAnim?.stop()
		demoBox.getAnimations().forEach((a) => a.cancel())
		demoBox.style.opacity = '0'
		demoBox.style.transform = 'translateX(-60px)'
		currentAnim = animate(
			demoBox,
			{ opacity: [0, 1], x: [-60, 0] },
			{ duration: 0.6, easing: 'ease-out' }
		)
	}

	function resetDemo() {
		currentAnim?.stop()
		if (!demoBox) return
		demoBox.getAnimations().forEach((a) => a.cancel())
		demoBox.style.opacity = '0'
		demoBox.style.transform = 'translateX(-60px)'
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			<code style="color: var(--purple); background: var(--surface); padding: 4px 14px; border-radius: 8px; font-size: 1.6rem;">motion.div</code>
			<span style="font-size: 1.2rem; color: var(--text-muted);"> — one prop</span>
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="tsx"
					theme="poimandres"
					code={`<motion.div
  animate={{ opacity: 1, x: 0 }}
/>`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="min-height: 220px;">
					<div
						bind:this={demoBox}
						style="width: 90px; height: 90px; border-radius: 16px; background: linear-gradient(135deg, var(--purple), var(--blue)); opacity: 0; transform: translateX(-60px); box-shadow: 0 8px 30px rgba(168, 139, 250, 0.3);"
					></div>
				</div>
			</div>
		</div>
	</Transition>

	<Action do={runMotionDemo} undo={resetDemo} />

	<Action
		do={async () => {
			resetDemo()
			await code.update`<motion.div
  initial={{ opacity: 0, x: -60 }}
  animate={{ opacity: 1, x: 0 }}
  transition={{ duration: 0.6 }}
/>`
		}}
		undo={async () => {
			await code.update`<motion.div
  animate={{ opacity: 1, x: 0 }}
/>`
		}}
	/>

	<Action
		do={async () => {
			runMotionDemo()
			await code.selectLines`*`
		}}
		undo={async () => {
			resetDemo()
			await code.selectLines`*`
		}}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			Compare: WAAPI needed 13 lines. Motion needs <span style="color: var(--purple); font-weight: 600;">3</span>.
		</p>
	</Transition>
</div>
