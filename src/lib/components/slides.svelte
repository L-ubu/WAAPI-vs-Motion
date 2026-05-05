<svelte:options runes />

<script lang="ts">
	import AnimotionSlide from './slide.svelte'
	import type { Component } from 'svelte'

	let { center = false } = $props()

	type SlideModule = {
		default: Component
		component?: Component
		props?: any
	}

	const allSlides = Object.entries(
		import.meta.glob<SlideModule>('/src/slides/**/slide.svelte', {
			eager: true
		})
	)
		.map(([filename, exports]) => {
			const matches = filename.match(/slides\/(?<number>\d+)\/slide.svelte/)
			return [+matches!.groups!.number, exports] as const
		})
		.sort(([a], [b]) => a - b)

	type SlideEntry = (typeof allSlides)[number]

	const chapters: SlideEntry[][] = []
	for (const slide of allSlides) {
		const isChapterStart = slide[1].props?.transition === 'slide'
		if (isChapterStart || chapters.length === 0) {
			chapters.push([slide])
		} else {
			chapters[chapters.length - 1].push(slide)
		}
	}
</script>

{#each chapters as chapter}
	{#if chapter.length === 1}
		{@const [_, Slide] = chapter[0]}
		{@const Wrapper = Slide.component ?? AnimotionSlide}
		{@const slideProps = {
			class: center ? 'h-full place-content-center place-items-center' : null,
			...(Slide.props ?? {})
		}}
		<Wrapper {...slideProps}>
			<Slide.default></Slide.default>
		</Wrapper>
	{:else}
		<section>
			{#each chapter as [_, Slide]}
				{@const Wrapper = Slide.component ?? AnimotionSlide}
				{@const slideProps = {
					class: center ? 'h-full place-content-center place-items-center' : null,
					...(Slide.props ?? {})
				}}
				<Wrapper {...slideProps}>
					<Slide.default></Slide.default>
				</Wrapper>
			{/each}
		</section>
	{/if}
{/each}
