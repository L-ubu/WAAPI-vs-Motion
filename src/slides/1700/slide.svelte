<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'
	import { animate } from 'motion'

	let code: ReturnType<typeof Code>
	let listItems = $state([
		{ id: 1, label: 'Item A', color: 'var(--purple)' },
		{ id: 2, label: 'Item B', color: 'var(--cyan)' },
		{ id: 3, label: 'Item C', color: 'var(--emerald)' },
	])
	let nextId = 4
	let itemEls: Record<number, HTMLDivElement> = {}

	function trackEl(node: HTMLDivElement, id: number) {
		itemEls[id] = node
		return {
			destroy() {
				delete itemEls[id]
			}
		}
	}

	function addItem() {
		const colors = ['var(--amber)', 'var(--pink)', 'var(--purple)', 'var(--cyan)', 'var(--emerald)']
		const newItem = {
			id: nextId++,
			label: `Item ${String.fromCharCode(64 + nextId - 1)}`,
			color: colors[Math.floor(Math.random() * colors.length)]
		}
		listItems = [...listItems, newItem]
		requestAnimationFrame(() => {
			const el = itemEls[newItem.id]
			if (el) {
				animate(el, { opacity: [0, 1], y: [20, 0], scale: [0.8, 1] }, { duration: 0.4, easing: 'ease-out' })
			}
		})
	}

	function removeItem() {
		if (listItems.length === 0) return
		const lastItem = listItems[listItems.length - 1]
		const el = itemEls[lastItem.id]
		if (el) {
			animate(el, { opacity: [1, 0], x: [0, 60], scale: [1, 0.8] }, { duration: 0.3, easing: 'ease-in' }).then(() => {
				listItems = listItems.filter((i) => i.id !== lastItem.id)
			})
		} else {
			listItems = listItems.slice(0, -1)
		}
	}
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			AnimatePresence
		</p>
	</Transition>

	<Transition>
		<div style="display: flex; gap: 2.5rem; align-items: center; width: 100%; max-width: 900px;">
			<div style="flex: 1;">
				<Code
					bind:this={code}
					lang="tsx"
					theme="poimandres"
					code={`<AnimatePresence>
  {items.map(item => (
    <motion.div
      key={item.id}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, x: 60 }}
    >
      {item.label}
    </motion.div>
  ))}
</AnimatePresence>`}
					options={{ duration: 600, stagger: 0.3, containerStyle: false }}
				/>
			</div>

			<div style="flex: 0.5;">
				<div class="demo-area" style="flex-direction: column; gap: 0.8rem; min-height: 260px; padding: 1.5rem;">
					<div style="display: flex; flex-direction: column; gap: 0.4rem; width: 100%; min-height: 120px;">
						{#each listItems as item (item.id)}
							<div
								use:trackEl={item.id}
								style="padding: 0.6rem 0.8rem; background: rgba(255,255,255,0.05); border: 1.5px solid {item.color}; border-radius: 10px; font-size: 0.8rem; color: {item.color}; font-family: var(--r-code-font); text-align: center; font-weight: 600;"
							>
								{item.label}
							</div>
						{/each}
					</div>
					<div style="display: flex; gap: 0.5rem;">
						<button class="demo-btn" onclick={addItem}>+ Add</button>
						<button class="demo-btn" onclick={removeItem}>- Remove</button>
					</div>
				</div>
			</div>
		</div>
	</Transition>

	<Action
		do={() => code.selectLines`7`}
		undo={() => code.selectLines`*`}
	/>

	<Action
		do={() => code.selectLines`*`}
		undo={() => code.selectLines`7`}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			The <code style="color: var(--purple); background: var(--surface); padding: 2px 8px; border-radius: 6px;">exit</code> prop — impossible with WAAPI alone
		</p>
	</Transition>
</div>
