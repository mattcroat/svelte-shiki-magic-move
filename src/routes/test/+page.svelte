<script lang="ts">
	import { onMount } from 'svelte'
	import { getHighlighter, type HighlighterCore } from 'shiki'
	import { codeToKeyedTokens, createMagicMoveMachine } from 'shiki-magic-move/core'
	import { MagicMoveRenderer } from 'shiki-magic-move/renderer'

	let container: HTMLPreElement
	let highlighter: HighlighterCore
	let code = `const hello = 'world'`
	let lang = 'ts'
	let theme = 'poimandres'
	let options = {}

	$: machine = createMagicMoveMachine(
		(code) => codeToKeyedTokens(highlighter, code, { lang, theme }),
		options
	)
	$: result = getResult(code, highlighter)
	$: renderer = getRenderer(container)
	$: render(result)

	onMount(async () => {
		highlighter = await getHighlighter({
			themes: ['poimandres'],
			langs: ['javascript', 'typescript'],
		})
	})

	async function render() {
		if (!result) return
		if (result.previous) renderer.replace(result.previous)
		await renderer.render(result.current)
	}

	function getResult() {
		if (!highlighter) return
		return machine.commit(code)
	}

	function getRenderer() {
		if (!container) return
		return new MagicMoveRenderer(container)
	}

	function animate() {
		code = `let hi = 'hello'`
	}
</script>

<pre bind:this={container} class="shiki-magic-move-container"></pre>
<button on:click={animate}>Animate</button>
