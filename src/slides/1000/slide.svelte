<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { Transition } from '$lib/index.js'

	const limitations = [
		{ label: 'No spring physics', desc: 'Only CSS easing functions' },
		{ label: 'No gesture system', desc: 'No hover/tap/drag built-in' },
		{ label: 'No layout animations', desc: 'Can\'t animate between layouts' },
		{ label: 'No exit animations', desc: 'Elements disappear instantly' },
		{ label: 'No declarative API', desc: 'Imperative .animate() calls only' },
		{ label: 'Manual sequencing', desc: 'Chain with promises yourself' },
	]
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;">
	<Transition visible>
		<p style="font-size: 2.2rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			WAAPI <span style="color: #ef4444;">Limitations</span>
		</p>
	</Transition>

	<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 0.8rem 2rem; max-width: 750px;">
		{#each limitations as item, i}
			<Transition>
				<div style="display: flex; align-items: flex-start; gap: 0.8rem; padding: 0.7rem 1rem; background: var(--surface); border: 1px solid var(--border); border-radius: 10px;">
					<span style="color: #ef4444; font-size: 1.1rem; flex-shrink: 0; margin-top: 2px;">✗</span>
					<div>
						<p style="font-size: 1rem; font-weight: 600; color: var(--text);">{item.label}</p>
						<p style="font-size: 0.8rem; color: var(--text-muted);">{item.desc}</p>
					</div>
				</div>
			</Transition>
		{/each}
	</div>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			This is where <span style="color: var(--purple); font-weight: 600;">Motion</span> comes in...
		</p>
	</Transition>
</div>
