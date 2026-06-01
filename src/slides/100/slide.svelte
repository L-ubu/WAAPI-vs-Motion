<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({ transition: 'slide' })
</script>

<script>
	import { Action } from '$lib/index.js'
	import { tween } from '@animotion/motion'

	let heroOpacity = tween(0, { duration: 600 })
	let boxesVisible = tween(0, { duration: 500 })
	let animating = $state(false)

	async function playAnimation() {
		await boxesVisible.to(1)
		animating = true
		await heroOpacity.to(1)
	}
</script>

<Action
	actions={[
		async () => { await playAnimation() },
	]}
/>

<div class="divider-slide" style="justify-content: center; gap: 0;">
	<div style="display: flex; gap: 3rem; align-items: center; margin-bottom: 2rem; opacity: {boxesVisible.current};">
		<!-- WAAPI box: linear keyframe animation -->
		<div style="display: flex; flex-direction: column; align-items: center; gap: 0.6rem;">
			<div class="hero-track">
				<div class="hero-box hero-box--waapi" class:animating></div>
			</div>
			<span style="font-family: var(--r-code-font); font-size: 0.65rem; color: var(--amber); letter-spacing: 0.05em;">WAAPI</span>
		</div>

		<span style="font-size: 1.2rem; color: var(--text-muted); font-weight: 300;">vs</span>

		<!-- Motion box: spring physics animation -->
		<div style="display: flex; flex-direction: column; align-items: center; gap: 0.6rem;">
			<div class="hero-track">
				<div class="hero-box hero-box--motion" class:animating></div>
			</div>
			<span style="font-family: var(--r-code-font); font-size: 0.65rem; color: var(--purple); letter-spacing: 0.05em;">Motion</span>
		</div>
	</div>

	<div style="display: flex; flex-direction: column; align-items: center; min-height: 220px; justify-content: flex-start; margin-top: 0.5rem;">
		<p style="font-size: 0.9rem; color: var(--text-muted); letter-spacing: 0.2em; text-transform: uppercase; opacity: {heroOpacity.current};">
			R&D Knowledge Sharing
		</p>
		<p
			class="gradient-text"
			style="font-size: 5rem; font-weight: 800; letter-spacing: -0.04em; line-height: 1.1; margin-top: 1rem; opacity: {heroOpacity.current};"
		>
			Web Animations
		</p>
		<p style="font-size: 1.8rem; color: var(--text-muted); margin-top: 1rem; font-weight: 300; opacity: {heroOpacity.current};">
			WAAPI <span style="color: var(--purple);">vs</span> Motion
		</p>
		<p style="font-size: 1rem; color: var(--text-muted); opacity: 0.6; margin-top: 0.8rem; opacity: {heroOpacity.current};">
			When to use the browser, when to reach for a library
		</p>
		<div style="margin-top: 2.5rem; display: flex; flex-direction: column; align-items: center; gap: 0.4rem; opacity: {heroOpacity.current};">
			<p style="font-size: 0.9rem; color: var(--text-muted);">
				Luca Vandenweghe
			</p>
		</div>
	</div>
</div>

<style>
	.hero-track {
		width: 160px;
		height: 48px;
		background: var(--surface);
		border: 1px solid var(--border);
		border-radius: 12px;
		position: relative;
		overflow: hidden;
		display: flex;
		align-items: center;
		padding: 0 8px;
	}

	.hero-box {
		width: 32px;
		height: 32px;
		border-radius: 8px;
		position: relative;
		left: 0;
	}

	.hero-box--waapi {
		background: linear-gradient(135deg, var(--amber), #f59e0b);
	}

	.hero-box--motion {
		background: linear-gradient(135deg, var(--purple), var(--blue));
	}

	.hero-box--waapi.animating {
		animation: slide-linear 2s cubic-bezier(0.4, 0, 0.2, 1) infinite;
	}

	.hero-box--motion.animating {
		animation: slide-spring 2s cubic-bezier(0.34, 1.56, 0.64, 1) infinite;
	}

	@keyframes slide-linear {
		0%, 100% { transform: translateX(0); }
		50% { transform: translateX(112px); }
	}

	@keyframes slide-spring {
		0%, 100% { transform: translateX(0); }
		40% { transform: translateX(120px); }
		50% { transform: translateX(108px); }
		60% { transform: translateX(114px); }
		70% { transform: translateX(112px); }
	}
</style>

<aside class="notes">
Welcome. Today we are comparing native browser animations (WAAPI) vs the Motion library.
Goal: know when to use which tool for the job.
</aside>
