<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({ transition: 'slide' })
</script>

<script>
	import { Transition } from '$lib/index.js'
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 2rem;">
	<Transition visible>
		<p class="gradient-text" style="font-size: 4.5rem; font-weight: 800; letter-spacing: -0.04em;">
			Questions?
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; align-items: center; gap: 3rem;">
			<div style="display: flex; flex-direction: column; gap: 1.2rem;">
				<div style="display: flex; flex-direction: column; gap: 0.3rem;">
					<p style="font-size: 1.4rem; font-weight: 700; color: var(--text);">Luca Vandenweghe</p>
					<p style="font-size: 1rem; color: var(--purple); font-weight: 500;">React Developer @ iO Digital</p>
				</div>

				<div class="stagger-children" style="display: flex; flex-direction: column; gap: 0.5rem;">
					<div style="display: flex; align-items: center; gap: 0.8rem;">
						<span style="color: var(--blue); font-size: 1rem; min-width: 20px;">@</span>
						<span style="font-size: 0.95rem; color: var(--text);">luca.vandenweghe@iodigital.com</span>
					</div>
					<div style="display: flex; align-items: center; gap: 0.8rem;">
						<span style="color: var(--emerald); font-size: 1rem; min-width: 20px;">⌂</span>
						<span style="font-size: 0.95rem; color: var(--text);">github.com/L-ubu</span>
					</div>
					<div style="display: flex; align-items: center; gap: 0.8rem;">
						<span style="color: var(--cyan); font-size: 1rem; min-width: 20px;">♦</span>
						<span style="font-size: 0.95rem; color: var(--text);">Slack: Luca Vandenweghe</span>
					</div>
				</div>

				<div style="display: flex; flex-direction: column; gap: 0.3rem; margin-top: 0.5rem;">
					<p style="font-size: 0.85rem; color: var(--text-muted);">Useful links</p>
					<div style="display: flex; flex-direction: column; gap: 0.25rem; font-family: var(--r-code-font); font-size: 0.85rem;">
						<span style="color: var(--amber);">developer.mozilla.org/Web_Animations_API</span>
						<span style="color: var(--purple);">motion.dev</span>
						<span style="color: var(--cyan);">motion.dev/docs/react-quick-start</span>
					</div>
				</div>
			</div>

			<div style="display: flex; flex-direction: column; align-items: center; gap: 0.6rem;">
				<div style="background: var(--surface); border: 1px solid var(--border); border-radius: 14px; padding: 1rem;">
					<img src="/qr-github.png" alt="QR code to GitHub" style="width: 160px; height: 160px; border-radius: 8px;" />
				</div>
				<p style="font-size: 0.8rem; color: var(--text-muted);">Scan for GitHub</p>
			</div>
		</div>
	</Transition>

	<Transition>
		<p style="font-size: 1.5rem; color: var(--text); font-weight: 600; margin-top: 0.5rem;">
			Thanks for listening!
		</p>
	</Transition>
</div>

<aside class="notes">
Open for questions. QR code links to the repo with all code and slides.
</aside>
