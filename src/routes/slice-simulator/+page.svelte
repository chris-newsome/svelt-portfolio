<script>
	import { SliceSimulator } from '@slicemachine/adapter-sveltekit/simulator';
	import { SliceZone } from '@prismicio/svelte';
	import { components } from '$lib/slices';
</script>

<SliceSimulator let:slices background="#faf8f3">  <!-- Light pastel gray background -->
	<SliceZone {slices} {components} />
</SliceSimulator>
