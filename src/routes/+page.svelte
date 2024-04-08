<script lang="ts">
	import ShikiMagicMove from '$lib/ShikiMagicMove.svelte'
	import { getHighlighter } from 'shiki'

	let highlighter = getHighlighter({
		themes: ['poimandres'],
		langs: ['svelte'],
	})

	let code = $state(
		`
<script>
  let count = 0
  $: double = count * 2
<\/script>

<button on:click={() => count++}>
  {double}
</button>
`.trim()
	)

	function animate() {
		code = `
<script>
  let count = $state(0)
  let double = $derived(count * 2)
<\/script>

<button onclick={() => count++}>
  {double}
</button>
`.trim()
	}
</script>

{#await highlighter then highlighter}
	<ShikiMagicMove
		lang="svelte"
		theme="poimandres"
		options={{ duration: 1000, stagger: 1 }}
		onStart={() => console.log('start')}
		onEnd={() => console.log('end')}
		{highlighter}
		{code}
	/>
{/await}

<button on:click={animate}>Animate</button>
