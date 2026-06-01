<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let code: ReturnType<typeof Code>
	let gestureBox: HTMLDivElement

	function setupHover() {
		if (!gestureBox) return
		gestureBox.onmouseenter = () => {
			animate(gestureBox, { scale: 1.15, backgroundColor: 'rgba(168, 139, 250, 0.25)' }, { type: 'spring', stiffness: 400, damping: 15 })
		}
		gestureBox.onmouseleave = () => {
			animate(gestureBox, { scale: 1, backgroundColor: 'rgba(168, 139, 250, 0.1)' }, { type: 'spring', stiffness: 400, damping: 15 })
		}
	}

	function setupTap() {
		if (!gestureBox) return
		gestureBox.onpointerdown = () => {
			animate(gestureBox, { scale: 0.9 }, { type: 'spring', stiffness: 400, damping: 15 })
		}
		gestureBox.onpointerup = () => {
			animate(gestureBox, { scale: 1 }, { type: 'spring', stiffness: 400, damping: 15 })
		}
	}

	let isDragging = false
	let startX = 0
	let startY = 0
	let currentX = 0
	let currentY = 0

	function setupDrag() {
		if (!gestureBox) return
		gestureBox.style.cursor = 'grab'
		gestureBox.onpointerdown = (e) => {
			isDragging = true
			gestureBox.getAnimations().forEach((a) => a.cancel())
			startX = e.clientX - currentX
			startY = e.clientY - currentY
			gestureBox.style.cursor = 'grabbing'
			gestureBox.setPointerCapture(e.pointerId)
		}
		gestureBox.onpointermove = (e) => {
			if (!isDragging) return
			currentX = e.clientX - startX
			currentY = e.clientY - startY
			gestureBox.style.transform = `translate(${currentX}px, ${currentY}px)`
		}
		gestureBox.onpointerup = () => {
			isDragging = false
			gestureBox.style.cursor = 'grab'
			gestureBox.style.transform = ''
			animate(
				gestureBox,
				{ transform: [`translate(${currentX}px, ${currentY}px)`, 'translate(0px, 0px)'] },
				{ type: 'spring', stiffness: 300, damping: 20 }
			)
			currentX = 0
			currentY = 0
		}
	}

	function resetGestures() {
		if (!gestureBox) return
		gestureBox.onmouseenter = null
		gestureBox.onmouseleave = null
		gestureBox.onpointerdown = null
		gestureBox.onpointerup = null
		gestureBox.onpointermove = null
		gestureBox.style.cursor = 'default'
		gestureBox.style.transform = ''
		gestureBox.getAnimations().forEach((a) => a.cancel())
		currentX = 0
		currentY = 0
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			Gestures
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="tsx"
					theme="poimandres"
					code={`<motion.div
  whileHover={{ scale: 1.15 }}
/>`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="min-height: 220px;">
					<div
						bind:this={gestureBox}
						style="width: 100px; height: 100px; border-radius: 18px; background: rgba(168, 139, 250, 0.1); border: 2px solid var(--purple); display: flex; align-items: center; justify-content: center; font-size: 0.75rem; color: var(--purple); font-family: var(--r-code-font); user-select: none; touch-action: none; font-weight: 600;"
					>
						hover me
					</div>
				</div>
			</div>
		</div>
	</Transition>

	<Action
		do={() => {
			setupHover()
			code.selectLines`2`
		}}
		undo={() => {
			resetGestures()
			code.selectLines`*`
		}}
	/>

	<Action
		do={async () => {
			resetGestures()
			await code.update`<motion.div
  whileHover={{ scale: 1.15 }}
  whileTap={{ scale: 0.9 }}
/>`
			setupHover()
			setupTap()
			gestureBox.textContent = 'click me'
		}}
		undo={async () => {
			resetGestures()
			await code.update`<motion.div
  whileHover={{ scale: 1.15 }}
/>`
			gestureBox.textContent = 'hover me'
		}}
	/>

	<Action
		do={async () => {
			resetGestures()
			await code.update`<motion.div
  whileHover={{ scale: 1.15 }}
  whileTap={{ scale: 0.9 }}
  drag
  dragConstraints={{
    left: -50, right: 50,
    top: -50, bottom: 50
  }}
/>`
			setupHover()
			setupTap()
			setupDrag()
			gestureBox.textContent = 'drag me'
		}}
		undo={async () => {
			resetGestures()
			await code.update`<motion.div
  whileHover={{ scale: 1.15 }}
  whileTap={{ scale: 0.9 }}
/>`
			setupHover()
			setupTap()
			gestureBox.textContent = 'click me'
		}}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			Hover, tap, drag - <span style="color: var(--purple); font-weight: 600;">declarative</span>, not imperative
		</p>
	</Transition>
</div>

<aside class="notes">
Declarative gesture handling. One prop each.
No addEventListener, no cleanup, no pointer capture math.
Drag has constraints built in. Demo: try hovering, clicking, dragging.
</aside>
