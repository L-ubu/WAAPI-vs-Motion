<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { Transition } from '$lib/index.js'

	const stats = [
		{ value: '96.4%', label: 'Browser support', color: 'var(--emerald)' },
		{ value: '0kb', label: 'Bundle size', color: 'var(--amber)' },
		{ value: 'GPU', label: 'Compositor thread', color: 'var(--cyan)' },
	]
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;">
	<Transition visible>
		<p style="font-size: 2.2rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			What is the <span style="color: var(--amber);">Web Animations API</span>?
		</p>
	</Transition>

	<Transition>
		<p style="font-size: 1.15rem; color: var(--text-muted); max-width: 650px; text-align: center;">
			A native browser API to create and control animations from JavaScript — no libraries needed.
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2rem; margin-top: 1rem;">
			{#each stats as stat}
				<div style="display: flex; flex-direction: column; align-items: center; gap: 0.5rem; padding: 1.5rem 2.5rem; background: var(--surface); border: 1px solid var(--border); border-radius: 14px; min-width: 160px;">
					<span style="font-size: 2rem; font-weight: 800; color: {stat.color}; font-family: var(--r-code-font);">{stat.value}</span>
					<span style="font-size: 0.9rem; color: var(--text-muted);">{stat.label}</span>
				</div>
			{/each}
		</div>
	</Transition>

	<Transition>
		<div style="display: flex; flex-direction: column; gap: 0.6rem; font-size: 1.05rem; color: var(--text-muted); margin-top: 0.5rem;">
			<p>Replaces CSS transitions & <code style="color: var(--amber); background: var(--surface); padding: 2px 8px; border-radius: 6px;">requestAnimationFrame</code> loops</p>
			<p>Ships in every modern browser since 2020</p>
		</div>
	</Transition>
</div>
