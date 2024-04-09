<script lang="ts">
	import './app.css'

  let { children } = $props()
</script>

<svelte:head>
  <title>Shiki Magic Move</title>
</svelte:head>

<main>
  {@render children()}
</main>

<style>
  :global(*) {
    margin: 0px;
    padding: 0px;
  }

  :global(body) {
    height: 100svh;
    display:grid;
    place-content: center;
    font-family: 'Monaspace Neon', monospace;
    background-color: oklch(23.66% 0.02 271.79);
  }

  :global(pre) {
		width: 260px;
	}

  :global(button) {
    margin-block-start: 3rem;
    padding: 0.5rem;
    border-radius: 4px;
    border:none;
  }

  main {
    display:grid;
    place-items: center;
  }
</style>
