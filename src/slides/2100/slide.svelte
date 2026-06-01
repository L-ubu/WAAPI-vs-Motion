<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let waapiBall: HTMLDivElement
	let motionBall: HTMLDivElement
	let motionAnim: any = null

	function dropWaapi() {
		if (!waapiBall) return
		waapiBall.getAnimations().forEach((a) => a.cancel())
		waapiBall.animate(
			[{ transform: 'translateY(-120px)' }, { transform: 'translateY(0)' }],
			{ duration: 500, easing: 'cubic-bezier(0.25, 0.46, 0.45, 0.94)', fill: 'forwards' }
		)
	}

	function dropMotion() {
		if (!motionBall) return
		motionAnim?.stop()
		motionBall.getAnimations().forEach((a) => a.cancel())
		motionAnim = animate(motionBall, { y: [-120, 0] }, { type: 'spring', stiffness: 200, damping: 10 })
	}

	function resetBalls() {
		motionAnim?.stop()
		;[waapiBall, motionBall].forEach((el) => {
			if (!el) return
			el.getAnimations().forEach((a) => a.cancel())
			el.style.transform = 'translateY(-120px)'
		})
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Spring - what <span style="color: var(--amber);">WAAPI</span> can't do
		</p>
	</Transition>

	<div style="display: flex; gap: 3rem; align-items: stretch; width: 100%; max-width: 800px;">
		<div class="panel">
			<div class="panel-header" style="color: var(--amber);">WAAPI</div>
			<div class="panel-body" style="flex-direction: column; gap: 1rem; min-height: 240px;">
				<code style="font-size: 0.65rem; color: var(--text-muted); font-family: var(--r-code-font); background: rgba(255,255,255,0.03); padding: 0.5rem 0.8rem; border-radius: 8px; border: 1px solid var(--border); align-self: stretch; text-align: center;">easing: 'cubic-bezier(...)'</code>
				<div style="position: relative; width: 100%; height: 150px; display: flex; align-items: flex-end; justify-content: center;">
					<div style="position: absolute; left: 50%; top: 0; width: 1px; height: 100%; background: linear-gradient(to bottom, transparent, var(--border));"></div>
					<div bind:this={waapiBall} style="width: 32px; height: 32px; border-radius: 50%; background: linear-gradient(135deg, var(--amber), #f59e0b); box-shadow: 0 0 20px rgba(251, 191, 36, 0.3); transform: translateY(-120px);"></div>
					<div style="position: absolute; bottom: 0; width: 80px; height: 2px; background: var(--border); border-radius: 1px;"></div>
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted); text-align: center;">Smooth but no bounce</p>
			</div>
		</div>

		<div class="panel">
			<div class="panel-header" style="color: var(--purple);">Motion</div>
			<div class="panel-body" style="flex-direction: column; gap: 1rem; min-height: 240px;">
				<code style="font-size: 0.65rem; color: var(--text-muted); font-family: var(--r-code-font); background: rgba(255,255,255,0.03); padding: 0.5rem 0.8rem; border-radius: 8px; border: 1px solid var(--border); align-self: stretch; text-align: center;">type: "spring"</code>
				<div style="position: relative; width: 100%; height: 150px; display: flex; align-items: flex-end; justify-content: center;">
					<div style="position: absolute; left: 50%; top: 0; width: 1px; height: 100%; background: linear-gradient(to bottom, transparent, var(--border));"></div>
					<div bind:this={motionBall} style="width: 32px; height: 32px; border-radius: 50%; background: linear-gradient(135deg, var(--purple), var(--blue)); box-shadow: 0 0 20px rgba(168, 139, 250, 0.3); transform: translateY(-120px);"></div>
					<div style="position: absolute; bottom: 0; width: 80px; height: 2px; background: var(--border); border-radius: 1px;"></div>
				</div>
				<p style="font-size: 0.7rem; color: var(--text-muted); text-align: center;">Natural spring bounce</p>
			</div>
		</div>
	</div>

	<Action
		do={() => { dropWaapi(); dropMotion() }}
		undo={resetBalls}
	/>

	<Action
		do={() => { resetBalls(); setTimeout(() => { dropWaapi(); dropMotion() }, 200) }}
		undo={resetBalls}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			CSS easing can approximate - but a real spring <span style="color: var(--purple); font-weight: 600;">feels</span> different
		</p>
	</Transition>
</div>

<aside class="notes">
WAAPI only has cubic-bezier or steps - no real spring.
You can approximate with JS-computed keyframes but it is hacky.
Motion has a real spring physics solver built in.
</aside>
