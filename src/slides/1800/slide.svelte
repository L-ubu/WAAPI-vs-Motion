<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let code: ReturnType<typeof Code>
	let staggerItems: HTMLDivElement[] = []

	function runStagger() {
		requestAnimationFrame(() => {
			staggerItems.forEach((el, i) => {
				if (!el) return
				el.getAnimations().forEach((a) => a.cancel())
				animate(
					el,
					{ opacity: [0, 1], y: [30, 0], scale: [0.8, 1] },
					{ duration: 0.5, delay: i * 0.1, easing: 'ease-out' }
				)
			})
		})
	}

	function resetStagger() {
		staggerItems.forEach((el) => {
			if (!el) return
			el.getAnimations().forEach((a) => a.cancel())
			el.style.opacity = '0'
			el.style.transform = 'translateY(30px) scale(0.8)'
		})
	}

	const labels = ['Dashboard', 'Settings', 'Profile', 'Notifications', 'Analytics']
	const colors = ['var(--purple)', 'var(--cyan)', 'var(--amber)', 'var(--emerald)', 'var(--pink)']
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Variants & Stagger
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="tsx"
					theme="poimandres"
					code={`const parent = {
  visible: {
    transition: { staggerChildren: 0.1 }
  }
}

const child = {
  hidden: { opacity: 0, y: 30 },
  visible: { opacity: 1, y: 0 }
}

<motion.ul variants={parent} animate="visible">
  {items.map(item => (
    <motion.li key={item} variants={child} />
  ))}
</motion.ul>`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="flex-direction: column; gap: 0.6rem; min-height: 280px; padding: 1.5rem;">
					<div style="display: flex; flex-direction: column; gap: 0.4rem; width: 100%;">
						{#each labels as label, i}
							<div
								bind:this={staggerItems[i]}
								style="padding: 0.6rem 0.8rem; background: rgba(255,255,255,0.05); border: 1.5px solid {colors[i]}; border-radius: 10px; font-size: 0.8rem; color: {colors[i]}; font-family: var(--r-code-font); text-align: center; font-weight: 600; opacity: 0; transform: translateY(30px) scale(0.8);"
							>
								{label}
							</div>
						{/each}
					</div>
					<button class="demo-btn" onclick={() => { resetStagger(); setTimeout(runStagger, 100) }}>Replay</button>
				</div>
			</div>
		</div>
	</Transition>

	<Action do={runStagger} undo={resetStagger} />

	<Action
		do={() => code.selectLines`3`}
		undo={() => code.selectLines`*`}
	/>

	<Action
		do={() => code.selectLines`*`}
		undo={() => code.selectLines`3`}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			Remember the WAAPI <code style="color: var(--amber); background: var(--surface); padding: 2px 8px; border-radius: 6px;">forEach</code>? One line: <code style="color: var(--purple); background: var(--surface); padding: 2px 8px; border-radius: 6px;">staggerChildren: 0.1</code>
		</p>
	</Transition>
</div>
