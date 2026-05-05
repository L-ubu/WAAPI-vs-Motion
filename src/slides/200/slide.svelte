<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { Transition } from '$lib/index.js'
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			What we'll cover
		</p>
	</Transition>

	<div style="display: flex; flex-direction: column; gap: 1.2rem; min-width: 400px;">
		<Transition>
			<div style="display: flex; align-items: center; gap: 1rem; padding: 0.8rem 1.2rem; background: var(--surface); border: 1px solid var(--border); border-radius: 12px;">
				<span style="font-family: var(--r-code-font); font-size: 0.8rem; color: var(--amber); font-weight: 700;">01</span>
				<span style="font-size: 1.2rem; color: var(--text);">Web Animations API</span>
				<span style="font-size: 0.75rem; color: var(--text-muted); margin-left: auto;">native &middot; 0kb</span>
			</div>
		</Transition>

		<Transition>
			<div style="display: flex; align-items: center; gap: 1rem; padding: 0.8rem 1.2rem; background: var(--surface); border: 1px solid var(--border); border-radius: 12px;">
				<span style="font-family: var(--r-code-font); font-size: 0.8rem; color: var(--purple); font-weight: 700;">02</span>
				<span style="font-size: 1.2rem; color: var(--text);">Motion Library</span>
				<span style="font-size: 0.75rem; color: var(--text-muted); margin-left: auto;">React &middot; ~18kb</span>
			</div>
		</Transition>

		<Transition>
			<div style="display: flex; align-items: center; gap: 1rem; padding: 0.8rem 1.2rem; background: var(--surface); border: 1px solid var(--border); border-radius: 12px;">
				<span style="font-family: var(--r-code-font); font-size: 0.8rem; color: var(--cyan); font-weight: 700;">03</span>
				<span style="font-size: 1.2rem; color: var(--text);">Side by Side</span>
				<span style="font-size: 0.75rem; color: var(--text-muted); margin-left: auto;">live demos</span>
			</div>
		</Transition>

		<Transition>
			<div style="display: flex; align-items: center; gap: 1rem; padding: 0.8rem 1.2rem; background: var(--surface); border: 1px solid var(--border); border-radius: 12px;">
				<span style="font-family: var(--r-code-font); font-size: 0.8rem; color: var(--emerald); font-weight: 700;">04</span>
				<span style="font-size: 1.2rem; color: var(--text);">Verdict</span>
				<span style="font-size: 0.75rem; color: var(--text-muted); margin-left: auto;">when to use what</span>
			</div>
		</Transition>
	</div>
</div>
