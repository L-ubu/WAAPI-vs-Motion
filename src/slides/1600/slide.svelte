<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let code: ReturnType<typeof Code>
	let items = $state([
		{ id: 1, label: 'Alpha', color: 'var(--amber)' },
		{ id: 2, label: 'Beta', color: 'var(--cyan)' },
		{ id: 3, label: 'Gamma', color: 'var(--purple)' },
		{ id: 4, label: 'Delta', color: 'var(--emerald)' },
	])
	let itemEls: Record<number, HTMLDivElement> = {}

	function trackEl(node: HTMLDivElement, id: number) {
		itemEls[id] = node
		return { destroy() { delete itemEls[id] } }
	}

	function shuffleWithAnimation() {
		const positions: Record<number, DOMRect> = {}
		for (const item of items) {
			const el = itemEls[item.id]
			if (el) positions[item.id] = el.getBoundingClientRect()
		}

		const shuffled = [...items]
		for (let i = shuffled.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
		}
		items = shuffled

		requestAnimationFrame(() => {
			for (const item of items) {
				const el = itemEls[item.id]
				if (!el || !positions[item.id]) continue
				const newPos = el.getBoundingClientRect()
				const deltaY = positions[item.id].top - newPos.top
				if (Math.abs(deltaY) > 1) {
					animate(el, { y: [deltaY, 0] }, { type: 'spring', stiffness: 300, damping: 25 })
				}
			}
		})
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Layout Animations
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="tsx"
					theme="poimandres"
					code={`<motion.div layout>
  {items.map(item => (
    <motion.div
      key={item.id}
      layout
      transition={{
        type: "spring",
        stiffness: 300,
        damping: 25
      }}
    >
      {item.label}
    </motion.div>
  ))}
</motion.div>`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="flex-direction: column; gap: 0.8rem; min-height: 260px; padding: 1.5rem;">
					<div style="display: flex; flex-direction: column; gap: 0.5rem; width: 100%;">
						{#each items as item (item.id)}
							<div
								use:trackEl={item.id}
								style="padding: 0.7rem 1rem; background: rgba(255,255,255,0.05); border: 1.5px solid {item.color}; border-radius: 10px; font-size: 0.85rem; color: {item.color}; font-family: var(--r-code-font); text-align: center; font-weight: 600;"
							>
								{item.label}
							</div>
						{/each}
					</div>
					<button class="demo-btn" onclick={shuffleWithAnimation}>Shuffle</button>
				</div>
			</div>
		</div>
	</Transition>

	<Action
		do={() => code.selectLines`1,5`}
		undo={() => code.selectLines`*`}
	/>

	<Action
		do={() => {
			shuffleWithAnimation()
			code.selectLines`*`
		}}
		undo={() => code.selectLines`1,5`}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			One prop - <code style="color: var(--purple); background: var(--surface); padding: 2px 8px; border-radius: 6px;">layout</code> - handles all the FLIP math
		</p>
	</Transition>
</div>

<aside class="notes">
Motion killer feature. Add layout prop and it auto-animates any layout change using FLIP.
Position, size, even between different parents. No manual FLIP math needed.
</aside>
