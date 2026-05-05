<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let code: ReturnType<typeof Code>
	let ball: HTMLDivElement
	let currentAnim: any = null

	function dropBall(stiffness = 200, damping = 12) {
		if (!ball) return
		currentAnim?.stop()
		ball.getAnimations().forEach((a) => a.cancel())
		currentAnim = animate(
			ball,
			{ y: [-140, 0] },
			{ type: 'spring', stiffness, damping }
		)
	}

	function resetBall() {
		currentAnim?.stop()
		if (!ball) return
		ball.getAnimations().forEach((a) => a.cancel())
		ball.style.transform = 'translateY(-140px)'
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Spring Physics
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
  animate={{ y: 0 }}
  transition={{
    type: "spring",
    stiffness: 200,
    damping: 12
  }}
/>`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="min-height: 260px; flex-direction: column; justify-content: flex-end; padding-bottom: 2rem;">
				<div style="position: relative; width: 100%; height: 200px; display: flex; flex-direction: column; align-items: center; justify-content: flex-end;">
					<div style="position: absolute; top: 0; width: 1px; height: 100%; background: linear-gradient(to bottom, transparent, var(--border));"></div>
					<div
						bind:this={ball}
						style="width: 40px; height: 40px; border-radius: 50%; background: linear-gradient(135deg, var(--cyan), var(--emerald)); box-shadow: 0 0 25px rgba(34, 211, 238, 0.4); transform: translateY(-140px);"
					></div>
					<div style="width: 80px; height: 2px; background: var(--border); margin-top: 0.5rem; border-radius: 1px;"></div>
				</div>
			</div>
		</div>
	</Transition>

	<Action
		do={() => dropBall(200, 12)}
		undo={resetBall}
	/>

	<Action
		do={async () => {
			resetBall()
			await code.update`<motion.div
  animate={{ y: 0 }}
  transition={{
    type: "spring",
    stiffness: 600,
    damping: 8
  }}
/>`
			dropBall(600, 8)
		}}
		undo={async () => {
			resetBall()
			await code.update`<motion.div
  animate={{ y: 0 }}
  transition={{
    type: "spring",
    stiffness: 200,
    damping: 12
  }}
/>`
		}}
	/>

	<Action
		do={async () => {
			resetBall()
			await code.update`<motion.div
  animate={{ y: 0 }}
  transition={{
    type: "spring",
    stiffness: 100,
    damping: 5
  }}
/>`
			dropBall(100, 5)
		}}
		undo={async () => {
			resetBall()
			await code.update`<motion.div
  animate={{ y: 0 }}
  transition={{
    type: "spring",
    stiffness: 600,
    damping: 8
  }}
/>`
		}}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			No CSS easing can replicate a real spring — this is Motion's <span style="color: var(--purple); font-weight: 600;">superpower</span>
		</p>
	</Transition>
</div>
