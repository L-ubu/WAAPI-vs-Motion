<script module>
	import { defineProps } from '$lib/index.js'
	export const props = defineProps({})
</script>

<script lang="ts">
	import { Code, Action, Transition } from '$lib/index.js'

	let code: ReturnType<typeof Code>
</script>

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; gap: 1.5rem;">
	<Transition visible>
		<p style="font-size: 1.8rem; font-weight: 700; color: var(--text); letter-spacing: -0.02em;">
			WAAPI in <span style="color: var(--cyan);">React</span>
		</p>
	</Transition>

	<Transition>
		<Code
			bind:this={code}
			lang="tsx"
			theme="poimandres"
			code={`function FadeIn({ children }) {
  const ref = useRef(null)

  useEffect(() => {
    ref.current.animate(
      [
        { opacity: 0, transform: 'translateY(20px)' },
        { opacity: 1, transform: 'translateY(0)' }
      ],
      { duration: 500, easing: 'ease-out', fill: 'forwards' }
    )
  }, [])

  return <div ref={ref}>{children}</div>
}`}
			options={{ duration: 600, stagger: 0.3, containerStyle: false }}
		/>
	</Transition>

	<Action
		do={() => code.selectLines`2,4-12,13`}
		undo={() => code.selectLines`*`}
	/>

	<Action
		do={async () => {
			await code.update`function FadeIn({ children }) {
  const ref = useRef(null)

  // need useRef to access DOM
  // need useEffect for lifecycle
  // need to imperatively call .animate()
  // need manual cleanup for infinite animations
  useEffect(() => {
    const anim = ref.current.animate(
      [
        { opacity: 0, transform: 'translateY(20px)' },
        { opacity: 1, transform: 'translateY(0)' }
      ],
      { duration: 500, easing: 'ease-out', fill: 'forwards' }
    )
    return () => anim.cancel()
  }, [])

  return <div ref={ref}>{children}</div>
}`
		}}
		undo={async () => {
			await code.update`function FadeIn({ children }) {
  const ref = useRef(null)

  useEffect(() => {
    ref.current.animate(
      [
        { opacity: 0, transform: 'translateY(20px)' },
        { opacity: 1, transform: 'translateY(0)' }
      ],
      { duration: 500, easing: 'ease-out', fill: 'forwards' }
    )
  }, [])

  return <div ref={ref}>{children}</div>
}`
		}}
	/>

	<Action
		do={() => code.selectLines`4-7`}
		undo={() => code.selectLines`*`}
	/>

	<Transition>
		<p style="font-size: 1rem; color: var(--text-muted); margin-top: 0.5rem;">
			A lot of boilerplate for a simple fade-in
		</p>
	</Transition>
</div>

<aside class="notes">
Works in any framework - just need a ref to the DOM element.
No special bindings needed.
But notice - it is imperative code inside declarative components. Feels awkward.
</aside>
