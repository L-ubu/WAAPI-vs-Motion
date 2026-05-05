<script lang="ts">
	import { Presentation, Slides } from '$lib/index.js'
</script>

<Presentation
	options={{
		history: true,
		transition: 'slide',
		controls: false,
		progress: true,
		hash: true,
		center: true
	}}
>
	<Slides center={true} />
</Presentation>
