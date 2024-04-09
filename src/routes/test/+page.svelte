<script lang="ts">
	import { onMount, tick } from 'svelte'
	import type { BundledLanguage, SpecialLanguage } from 'shiki'
	import { codeToKeyedTokens, createMagicMoveMachine } from 'shiki-magic-move/core'
	import { MagicMoveRenderer } from 'shiki-magic-move/renderer'
	import type { MagicMoveDifferOptions, MagicMoveRenderOptions } from 'shiki-magic-move/types'
	import highlighter from './highlighter'

	export let code = 'let bool;'
	export let lang: BundledLanguage | SpecialLanguage = 'ts'
	export let theme = 'poimandres'
	export let options: MagicMoveRenderOptions & MagicMoveDifferOptions = {
		duration: 1000,
		stagger: 3,
	}

	let container: HTMLPreElement
	let machine: ReturnType<typeof createMagicMoveMachine>
	let renderer: MagicMoveRenderer

	onMount(async () => {
		machine = createMagicMoveMachine(
			(code) => codeToKeyedTokens(highlighter, code, { lang, theme }),
			options
		)
		renderer = new MagicMoveRenderer(container)
	})

	async function render() {
		const result = machine.commit(code)
		Object.assign(renderer.options, options)
		if (result.previous) renderer.replace(result.previous)
		await renderer.render(result.current)
	}

	$: if (highlighter && machine && code) {
		render()
	}
</script>

<pre bind:this={container} class="shiki-magic-move-container"></pre>
<button
	on:click={() => {
		code = 'let bool = true;'
	}}
>
	Animate
</button>
