<script lang="ts">
	import { onMount } from 'svelte'
	import type { BundledLanguage, SpecialLanguage } from 'shiki'
	import { codeToKeyedTokens, createMagicMoveMachine } from 'shiki-magic-move/core'
	import { MagicMoveRenderer } from 'shiki-magic-move/renderer'
	import type { MagicMoveDifferOptions, MagicMoveRenderOptions } from 'shiki-magic-move/types'
	import highlighter from './highlighter'

	export let code = `
<script>
  let count = 0
  $: double = count * 2
<\/script>

<button on:click={() => count++}>
  {double}
</button>
	`.trim()
	export let lang: BundledLanguage | SpecialLanguage = 'svelte'
	export let theme = 'poimandres'
	export let options: MagicMoveRenderOptions & MagicMoveDifferOptions = {
		duration: 2000,
		stagger: 3,
		// animateContainer: false,
		containerStyle: false,
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
		Object.assign(renderer.options, options)
		const result = machine.commit(code)
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
		code = `
<script>
  let count = $state(0)
  let double = $derived(count * 2)
<\/script>

<button onclick={() => count++}>
  {double}
</button>
`.trim()
	}}
>
	Animate
</button>
