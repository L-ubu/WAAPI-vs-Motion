<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let waapiBox: HTMLDivElement
	let motionBox: HTMLDivElement

	function setupWaapiHover() {
		if (!waapiBox) return
		waapiBox.onmouseenter = () => {
			waapiBox.animate([{ transform: 'scale(1.15)' }], { duration: 200, fill: 'forwards', easing: 'ease-out' })
		}
		waapiBox.onmouseleave = () => {
			waapiBox.animate([{ transform: 'scale(1)' }], { duration: 200, fill: 'forwards', easing: 'ease-out' })
		}
	}

	function setupMotionHover() {
		if (!motionBox) return
		motionBox.onmouseenter = () => {
			animate(motionBox, { scale: 1.15 }, { type: 'spring', stiffness: 400, damping: 15 })
		}
		motionBox.onmouseleave = () => {
			animate(motionBox, { scale: 1 }, { type: 'spring', stiffness: 400, damping: 15 })
		}
	}

	function resetAll() {
		[waapiBox, motionBox].forEach((el) => {
			if (!el) return
			el.onmouseenter = null
			el.onmouseleave = null
			el.style.transform = ''
			el.getAnimations().forEach((a) => a.cancel())
		})
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Hover — <span style="color: var(--amber);">WAAPI</span> vs <span style="color: var(--purple);">Motion</span>
		</p>
	</Transition>

	<div style="display: flex; gap: 3rem; align-items: stretch; width: 100%; max-width: 800px;">
		<div class="panel">
			<div class="panel-header" style="color: var(--amber);">WAAPI</div>
			<div class="panel-body" style="flex-direction: column; gap: 1.5rem;">
				<code style="font-size: 0.65rem; color: var(--text-muted); white-space: pre; line-height: 1.5; font-family: var(--r-code-font); background: rgba(255,255,255,0.03); padding: 0.6rem 0.8rem; border-radius: 8px; border: 1px solid var(--border); align-self: stretch;">{'el.onmouseenter = () =>\n  el.animate(\n    [{ scale: 1.15 }],\n    { fill: "forwards" }\n  )'}</code>
				<div
					bind:this={waapiBox}
					style="width: 80px; height: 80px; border-radius: 14px; background: rgba(251, 191, 36, 0.12); border: 2px solid var(--amber); display: flex; align-items: center; justify-content: center; font-size: 0.7rem; color: var(--amber); font-family: var(--r-code-font); cursor: pointer;"
				>
					hover
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted);">Linear easing</p>
			</div>
		</div>

		<div class="panel">
			<div class="panel-header" style="color: var(--purple);">Motion</div>
			<div class="panel-body" style="flex-direction: column; gap: 1.5rem;">
				<code style="font-size: 0.65rem; color: var(--text-muted); white-space: pre; line-height: 1.5; font-family: var(--r-code-font); background: rgba(255,255,255,0.03); padding: 0.6rem 0.8rem; border-radius: 8px; border: 1px solid var(--border); align-self: stretch;">{'whileHover={{ scale: 1.15 }}'}</code>
				<div
					bind:this={motionBox}
					style="width: 80px; height: 80px; border-radius: 14px; background: rgba(168, 139, 250, 0.12); border: 2px solid var(--purple); display: flex; align-items: center; justify-content: center; font-size: 0.7rem; color: var(--purple); font-family: var(--r-code-font); cursor: pointer;"
				>
					hover
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted);">Spring physics</p>
			</div>
		</div>
	</div>

	<Action
		do={() => { setupWaapiHover(); setupMotionHover() }}
		undo={resetAll}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			Same result, <span style="color: var(--amber); font-weight: 600;">5 lines</span> vs <span style="color: var(--purple); font-weight: 600;">1 prop</span>
		</p>
	</Transition>
</div>
