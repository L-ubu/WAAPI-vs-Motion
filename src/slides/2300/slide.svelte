<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let waapiItems = $state(['A', 'B', 'C', 'D'])
	let motionItems = $state(['A', 'B', 'C', 'D'])
	let motionEls: Record<string, HTMLDivElement> = {}

	const itemColor: Record<string, string> = { A: 'var(--amber)', B: 'var(--cyan)', C: 'var(--purple)', D: 'var(--emerald)' }

	function trackMotionEl(node: HTMLDivElement, label: string) {
		motionEls[label] = node
		return { destroy() { delete motionEls[label] } }
	}

	function shuffleBoth() {
		const positions: Record<string, DOMRect> = {}
		for (const [label, el] of Object.entries(motionEls)) {
			if (el) positions[label] = el.getBoundingClientRect()
		}

		const shuffle = (arr: string[]) => {
			const a = [...arr]
			for (let i = a.length - 1; i > 0; i--) {
				const j = Math.floor(Math.random() * (i + 1));
				[a[i], a[j]] = [a[j], a[i]]
			}
			return a
		}

		const newOrder = shuffle(waapiItems)
		waapiItems = newOrder
		motionItems = newOrder

		requestAnimationFrame(() => {
			for (const [label, el] of Object.entries(motionEls)) {
				if (!el || !positions[label]) continue
				const newPos = el.getBoundingClientRect()
				const dy = positions[label].top - newPos.top
				if (Math.abs(dy) > 1) {
					animate(el, { y: [dy, 0] }, { type: 'spring', stiffness: 300, damping: 25 })
				}
			}
		})
	}

	function resetBoth() {
		waapiItems = ['A', 'B', 'C', 'D']
		motionItems = ['A', 'B', 'C', 'D']
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Layout — Motion's <span style="color: var(--purple);">killer feature</span>
		</p>
	</Transition>

	<div style="display: flex; gap: 3rem; align-items: stretch; width: 100%; max-width: 800px;">
		<div class="panel">
			<div class="panel-header" style="color: var(--amber);">WAAPI</div>
			<div class="panel-body" style="flex-direction: column; gap: 0.8rem;">
				<div style="display: flex; flex-direction: column; gap: 0.4rem; width: 100%; max-width: 160px;">
					{#each waapiItems as item (item)}
						<div style="padding: 0.5rem 0.8rem; background: rgba(255,255,255,0.05); border: 1.5px solid {itemColor[item]}; border-radius: 8px; font-size: 0.8rem; color: {itemColor[item]}; font-family: var(--r-code-font); text-align: center; font-weight: 600;">
							{item}
						</div>
					{/each}
				</div>
				<p style="font-size: 0.7rem; color: #ef4444;">Items teleport</p>
			</div>
		</div>

		<div class="panel">
			<div class="panel-header" style="color: var(--purple);">Motion</div>
			<div class="panel-body" style="flex-direction: column; gap: 0.8rem;">
				<div style="display: flex; flex-direction: column; gap: 0.4rem; width: 100%; max-width: 160px;">
					{#each motionItems as item (item)}
						<div
							use:trackMotionEl={item}
							style="padding: 0.5rem 0.8rem; background: rgba(255,255,255,0.05); border: 1.5px solid {itemColor[item]}; border-radius: 8px; font-size: 0.8rem; color: {itemColor[item]}; font-family: var(--r-code-font); text-align: center; font-weight: 600;"
						>
							{item}
						</div>
					{/each}
				</div>
				<p style="font-size: 0.7rem; color: var(--emerald);">Items glide smoothly</p>
			</div>
		</div>
	</div>

	<Action
		do={shuffleBoth}
		undo={resetBoth}
	/>

	<Action
		do={shuffleBoth}
		undo={resetBoth}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.3rem;">
			WAAPI has no concept of layout animation — <code style="color: var(--purple); background: var(--surface); padding: 2px 8px; border-radius: 6px;">layout</code> is Motion-only
		</p>
	</Transition>
</div>
