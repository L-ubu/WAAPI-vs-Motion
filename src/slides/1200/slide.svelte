<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { Transition } from '$lib/index.js'

	const stats = [
		{ value: '31.6k', label: 'GitHub stars', color: 'var(--purple)' },
		{ value: '~30M', label: 'Monthly npm downloads', color: 'var(--cyan)' },
		{ value: '~18kb', label: 'Bundle size (tree-shaken)', color: 'var(--pink)' },
	]
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;">
	<Transition visible>
		<p style="font-size: 2.2rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			What is <span style="color: var(--purple);">Motion</span>?
		</p>
	</Transition>

	<Transition>
		<p style="font-size: 1.15rem; color: var(--text-muted); max-width: 650px; text-align: center;">
			A production-grade animation library for the web - formerly known as Framer Motion.
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2rem; margin-top: 0.5rem;">
			{#each stats as stat}
				<div style="display: flex; flex-direction: column; align-items: center; gap: 0.5rem; padding: 1.5rem 2.5rem; background: var(--surface); border: 1px solid var(--border); border-radius: 14px; min-width: 160px;">
					<span style="font-size: 2rem; font-weight: 800; color: {stat.color}; font-family: var(--r-code-font);">{stat.value}</span>
					<span style="font-size: 0.9rem; color: var(--text-muted);">{stat.label}</span>
				</div>
			{/each}
		</div>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 1.5rem; margin-top: 0.5rem;">
			{#each ['React', 'Vue', 'Vanilla JS'] as fw, i}
				{@const colors = ['var(--cyan)', 'var(--emerald)', 'var(--amber)']}
				<span style="font-size: 0.95rem; color: {colors[i]}; background: var(--surface); padding: 6px 16px; border-radius: 8px; border: 1px solid var(--border);">{fw}</span>
			{/each}
		</div>
	</Transition>

	<Transition>
		<p style="font-size: 0.95rem; color: var(--text-muted); margin-top: 0.5rem;">
			Springs, gestures, layout animations, exit animations - all built-in
		</p>
	</Transition>
</div>

<aside class="notes">
Formerly Framer Motion - renamed to just Motion.
Production-grade, around 16kb gzipped. Used by major companies.
Supports React, Vue, vanilla JS.
</aside>
