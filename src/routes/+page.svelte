<script lang="ts">
	import ShikiMagicMove from '$lib/ShikiMagicMove.svelte'
	import { getHighlighter } from 'shiki'

	let highlighter = getHighlighter({
		themes: ['poimandres'],
		langs: ['javascript', 'typescript'],
	})
	let code = $state(`const hello = 'world'`)

	function animate() {
		code = `let hi = 'hello'`
	}
</script>

{#await highlighter then highlighter}
	<ShikiMagicMove
		lang="ts"
		theme="poimandres"
		onStart={() => console.log('start')}
		onEnd={() => console.log('end')}
		{highlighter}
		{code}
	/>
{/await}

<button on:click={animate}>Animate</button>
