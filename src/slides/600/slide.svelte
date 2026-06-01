<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'

	let code: ReturnType<typeof Code>
	let orbitEl: HTMLDivElement
	let animation: Animation | null = null

	function startOrbit() {
		if (!orbitEl) return
		animation = orbitEl.animate(
			[
				{ transform: 'rotate(0deg) translateX(55px) rotate(0deg)' },
				{ transform: 'rotate(360deg) translateX(55px) rotate(-360deg)' }
			],
			{ duration: 2000, iterations: Infinity, easing: 'linear' }
		)
	}

	function pauseOrbit() {
		animation?.pause()
	}

	function playOrbit() {
		animation?.play()
	}

	function reverseOrbit() {
		if (animation) animation.playbackRate *= -1
	}

	function speedOrbit() {
		if (animation) animation.playbackRate = 3
	}

	function resetOrbit() {
		animation?.cancel()
		animation = null
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Playback Control
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="js"
					theme="poimandres"
					code={`const anim = box.animate(keyframes, {
  duration: 2000,
  iterations: Infinity
})

anim.pause()
anim.play()
anim.reverse()
anim.playbackRate = 3`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="flex-direction: column; gap: 1.5rem; min-height: 260px;">
				<div style="position: relative; width: 140px; height: 140px;">
					<div style="position: absolute; inset: 0; border: 1px dashed var(--border); border-radius: 50%;"></div>
					<div style="position: absolute; top: 50%; left: 50%; width: 6px; height: 6px; border-radius: 50%; background: var(--text-muted); transform: translate(-50%, -50%); opacity: 0.5;"></div>
					<div bind:this={orbitEl} style="position: absolute; top: 50%; left: 50%; width: 20px; height: 20px; margin: -10px 0 0 -10px; border-radius: 50%; background: linear-gradient(135deg, var(--amber), #f59e0b); box-shadow: 0 0 16px rgba(251, 191, 36, 0.4);">
					</div>
				</div>

				<div style="display: flex; gap: 0.4rem; flex-wrap: wrap; justify-content: center;">
					<button class="demo-btn" onclick={pauseOrbit}>Pause</button>
					<button class="demo-btn" onclick={playOrbit}>Play</button>
					<button class="demo-btn" onclick={reverseOrbit}>Reverse</button>
					<button class="demo-btn" onclick={speedOrbit}>3x</button>
				</div>
			</div>
		</div>
	</Transition>

	<Action
		do={() => {
			startOrbit()
			code.selectLines`1-4`
		}}
		undo={() => {
			resetOrbit()
			code.selectLines`*`
		}}
	/>

	<Action
		do={() => {
			pauseOrbit()
			code.selectLines`6`
		}}
		undo={() => {
			playOrbit()
			code.selectLines`1-4`
		}}
	/>

	<Action
		do={() => {
			playOrbit()
			code.selectLines`7`
		}}
		undo={() => {
			pauseOrbit()
			code.selectLines`6`
		}}
	/>

	<Action
		do={() => {
			reverseOrbit()
			code.selectLines`8`
		}}
		undo={() => {
			reverseOrbit()
			code.selectLines`7`
		}}
	/>

	<Action
		do={() => {
			speedOrbit()
			code.selectLines`9`
		}}
		undo={() => {
			if (animation) animation.playbackRate = 1
			code.selectLines`8`
		}}
	/>
</div>

<aside class="notes">
Unlike CSS animations, WAAPI gives you an Animation object back.
You can pause, play, reverse, change playbackRate.
Great for interactive UIs where user controls playback.
</aside>
