<script>
	import Jogo from './jogar.svelte'
	import Sobre from './sobre.svelte'
	import Menu from './menu.svelte'
	import { estado } from './Estado.js'
  	import Comojogar from './comojogar.svelte';
</script>

{#if $estado === 'menu'}
	<Menu/>
{:else if $estado === 'sobre'}
	<Sobre/>
{:else if $estado === 'jogar'}
	<Jogo/>
{:else if $estado === 'comojogar'}
	<Comojogar/>
{/if}

