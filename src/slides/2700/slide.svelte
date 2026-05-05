<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { Transition } from '$lib/index.js'

	const waapiReasons = [
		'Simple transitions (fade, slide)',
		'Playback control needed',
		'Zero-bundle-size constraint',
		'Framework-agnostic code',
		'Scroll-driven animations',
	]

	const motionReasons = [
		'Spring physics / natural feel',
		'Gestures (hover, tap, drag)',
		'Layout animations',
		'Exit animations (AnimatePresence)',
		'React / Vue projects',
	]
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;">
	<Transition visible>
		<p style="font-size: 2rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			When to use what
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; width: 100%; max-width: 800px;">
			<div style="flex: 1;">
				<div style="background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 1.5rem;">
					<p style="font-size: 1.1rem; font-weight: 700; color: var(--amber); margin-bottom: 1rem;">Use WAAPI when...</p>
					<div style="display: flex; flex-direction: column; gap: 0.6rem;">
						{#each waapiReasons as reason}
							<div style="display: flex; align-items: center; gap: 0.6rem;">
								<span style="color: var(--amber); flex-shrink: 0;">→</span>
								<span style="font-size: 0.9rem; color: var(--text-muted);">{reason}</span>
							</div>
						{/each}
					</div>
				</div>
			</div>

			<div style="flex: 1;">
				<div style="background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 1.5rem;">
					<p style="font-size: 1.1rem; font-weight: 700; color: var(--purple); margin-bottom: 1rem;">Use Motion when...</p>
					<div style="display: flex; flex-direction: column; gap: 0.6rem;">
						{#each motionReasons as reason}
							<div style="display: flex; align-items: center; gap: 0.6rem;">
								<span style="color: var(--purple); flex-shrink: 0;">→</span>
								<span style="font-size: 0.9rem; color: var(--text-muted);">{reason}</span>
							</div>
						{/each}
					</div>
				</div>
			</div>
		</div>
	</Transition>

	<Transition>
		<p style="font-size: 0.95rem; color: var(--text-muted);">
			Fun fact: Motion uses WAAPI under the hood
		</p>
	</Transition>
</div>
