<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let waapiList = $state(['Item 1', 'Item 2', 'Item 3'])
	let motionList = $state(['Item 1', 'Item 2', 'Item 3'])
	let motionEls: Record<string, HTMLDivElement> = {}

	function trackEl(node: HTMLDivElement, label: string) {
		motionEls[label] = node
		return { destroy() { delete motionEls[label] } }
	}

	function removeWaapi() {
		if (waapiList.length === 0) return
		waapiList = waapiList.slice(0, -1)
	}

	function removeMotion() {
		if (motionList.length === 0) return
		const last = motionList[motionList.length - 1]
		const el = motionEls[last]
		if (el) {
			animate(el, { opacity: [1, 0], x: [0, 60], scale: [1, 0.8] }, { duration: 0.3, easing: 'ease-in' }).then(() => {
				motionList = motionList.filter((i) => i !== last)
			})
		} else {
			motionList = motionList.slice(0, -1)
		}
	}

	function resetLists() {
		waapiList = ['Item 1', 'Item 2', 'Item 3']
		motionList = ['Item 1', 'Item 2', 'Item 3']
	}

	const colors: Record<string, string> = { 'Item 1': 'var(--amber)', 'Item 2': 'var(--cyan)', 'Item 3': 'var(--purple)' }
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Exit — the <span style="color: #ef4444;">React problem</span>
		</p>
	</Transition>

	<div style="display: flex; gap: 3rem; align-items: stretch; width: 100%; max-width: 800px;">
		<div class="panel">
			<div class="panel-header" style="color: var(--amber);">WAAPI</div>
			<div class="panel-body" style="flex-direction: column; gap: 0.8rem; min-height: 200px; justify-content: space-between;">
				<div style="display: flex; flex-direction: column; gap: 0.4rem; width: 100%; max-width: 160px; min-height: 100px;">
					{#each waapiList as item (item)}
						<div style="padding: 0.5rem 0.8rem; background: rgba(255,255,255,0.05); border: 1.5px solid {colors[item] || 'var(--border)'}; border-radius: 8px; font-size: 0.8rem; color: {colors[item] || 'var(--text)'}; font-family: var(--r-code-font); text-align: center; font-weight: 600;">
							{item}
						</div>
					{/each}
				</div>
				<div style="display: flex; flex-direction: column; align-items: center; gap: 0.4rem;">
					<button class="demo-btn" onclick={removeWaapi}>Remove last</button>
					<p style="font-size: 0.7rem; color: #ef4444;">*poof* — gone instantly</p>
				</div>
			</div>
		</div>

		<div class="panel">
			<div class="panel-header" style="color: var(--purple);">Motion</div>
			<div class="panel-body" style="flex-direction: column; gap: 0.8rem; min-height: 200px; justify-content: space-between;">
				<div style="display: flex; flex-direction: column; gap: 0.4rem; width: 100%; max-width: 160px; min-height: 100px;">
					{#each motionList as item (item)}
						<div
							use:trackEl={item}
							style="padding: 0.5rem 0.8rem; background: rgba(255,255,255,0.05); border: 1.5px solid {colors[item] || 'var(--border)'}; border-radius: 8px; font-size: 0.8rem; color: {colors[item] || 'var(--text)'}; font-family: var(--r-code-font); text-align: center; font-weight: 600;"
						>
							{item}
						</div>
					{/each}
				</div>
				<div style="display: flex; flex-direction: column; align-items: center; gap: 0.4rem;">
					<button class="demo-btn" onclick={removeMotion}>Remove last</button>
					<p style="font-size: 0.7rem; color: var(--emerald);">Graceful exit animation</p>
				</div>
			</div>
		</div>
	</div>

	<Action
		do={() => { removeWaapi(); removeMotion() }}
		undo={resetLists}
	/>

	<Action
		do={() => { removeWaapi(); removeMotion() }}
		undo={resetLists}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.3rem;">
			React unmounts the DOM — WAAPI can't animate what doesn't exist
		</p>
	</Transition>
</div>
