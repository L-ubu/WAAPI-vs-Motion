<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script>
	import { Transition } from '$lib/index.js'

	const rows = [
		{ feature: 'Spring physics', waapi: false, motion: true },
		{ feature: 'Gestures (hover/tap/drag)', waapi: false, motion: true },
		{ feature: 'Layout animations', waapi: false, motion: true },
		{ feature: 'Exit animations', waapi: false, motion: true },
		{ feature: 'Scroll-driven', waapi: true, motion: true },
		{ feature: 'Playback control', waapi: true, motion: true },
		{ feature: 'Bundle size', waapi: '0kb', motion: '~18kb' },
		{ feature: 'Performance', waapi: 'Native', motion: 'Near-native' },
		{ feature: 'Declarative API', waapi: false, motion: true },
	]
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 2rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Feature Comparison
		</p>
	</Transition>

	<Transition>
		<div style="border: 1px solid var(--border); border-radius: 12px; overflow: hidden; background: var(--surface);">
			<table style="border-collapse: collapse; font-size: 0.9rem; width: 650px;">
				<thead>
					<tr style="border-bottom: 1px solid var(--border);">
						<th style="padding: 0.6rem 1rem; text-align: left; color: var(--text); font-weight: 600;"></th>
						<th style="padding: 0.6rem 1rem; text-align: center; color: var(--amber); font-weight: 600; width: 100px;">WAAPI</th>
						<th style="padding: 0.6rem 1rem; text-align: center; color: var(--purple); font-weight: 600; width: 100px;">Motion</th>
					</tr>
				</thead>
				<tbody>
					{#each rows as row, i}
						<tr style="border-bottom: {i < rows.length - 1 ? '1px solid var(--border)' : 'none'};">
							<td style="padding: 0.5rem 1rem; color: var(--text-muted);">{row.feature}</td>
							<td style="padding: 0.5rem 1rem; text-align: center;">
								{#if typeof row.waapi === 'boolean'}
									<span style="color: {row.waapi ? 'var(--emerald)' : '#ef4444'};">{row.waapi ? '✓' : '✗'}</span>
								{:else}
									<span style="color: var(--amber); font-family: var(--r-code-font); font-size: 0.8rem;">{row.waapi}</span>
								{/if}
							</td>
							<td style="padding: 0.5rem 1rem; text-align: center;">
								{#if typeof row.motion === 'boolean'}
									<span style="color: {row.motion ? 'var(--emerald)' : '#ef4444'};">{row.motion ? '✓' : '✗'}</span>
								{:else}
									<span style="color: var(--purple); font-family: var(--r-code-font); font-size: 0.8rem;">{row.motion}</span>
								{/if}
							</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</Transition>
</div>
