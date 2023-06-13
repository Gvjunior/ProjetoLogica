<script>
  import { estado } from "./Estado.js";
  import { trocarEstadoDoJogo } from "./Estado.js";
</script>

<svelte:head>
  <!-- <link rel="stylesheet" href="/styles/menu.css"> -->
</svelte:head>

<h1>Caça Palavras</h1>

<button class="menu" on:click={() => trocarEstadoDoJogo("jogar")}>
  Jogar
</button>

<br />

<button class="menu" on:click={() => trocarEstadoDoJogo("sobre")}>
  Sobre
</button>

<br />
<button class="menu" on:click={() => trocarEstadoDoJogo("comojogar")}>
  Como Jogar
</button>

<style>
</style>
