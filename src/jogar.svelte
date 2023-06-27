<script>
  import { bubble } from "svelte/internal";
  import { trocarEstadoDoJogo } from "./Estado.js";

  let tema = "Esporte";

  let puzzle = [
    ["B", "M", "T", "A", "I", "T", "J"],
    ["O", "E", "E", "O", "E", "O", "O"],
    ["L", "S", "N", "N", "G", "L", "A"],
    ["A", "A", "I", "O", "R", "O", "I"],
    ["P", "S", "S", "T", "I", "D", "R"],
  ];

  let palavras = ["bola", "tenis", "mesa", "jogo"];
  let palavrasRestantes = palavras.length;
  let palavrasEncontradas = [];

  let selecionePalavras = "";
  let selecioneLetras = [];

  function slecionarLetra(i, j) {
    if (selecioneLetras.length === 0) {
      selecioneLetras.push({ i, j });
    } else {
      let ultimasLetras = selecioneLetras[selecioneLetras.length - 1];
      let dx = Math.abs(ultimasLetras.i - i);
      let dy = Math.abs(ultimasLetras.j - j);
      let movimentoValido = (dx === 1 && dy === 0) || (dx === 0 && dy === 1);

      if (movimentoValido) {
        selecioneLetras.push({ i, j });
      } else {
        selecioneLetras = [];
        selecioneLetras.push({ i, j });
      }
    }

    let palavra = "";
    for (const { i, j } of selecioneLetras) {
      palavra += puzzle[i][j];
    }

    if (palavras.includes(palavra) && !palavrasEncontradas.includes(palavra)) {
      selecionePalavras = palavra;
      selecioneLetras = [];

      palavrasEncontradas.push(palavra);
      palavrasRestantes--;
    }
  }

  function checagemDeDigito(digito) {
    const palavraDigitada = digito.target.value.toLowerCase();

    if (palavras.includes(palavraDigitada) && !palavrasEncontradas.includes(palavraDigitada)) {
      selecionePalavras = palavraDigitada;
      selecioneLetras = [];

      palavrasEncontradas.push(palavraDigitada);
      palavrasRestantes--;
    } else {
      selecioneLetras = [];
    }
  }

  function todasPalavrasEncontradas() {
    return palavrasRestantes === 0;
  }
</script>

<main>
  <h2>Jogo Caça-Palavras - {tema}</h2>
  <div class="container">
    <div class="puzzle">
      {#each puzzle as linha, i}
        {#each linha as coluna, j}
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <div
            class="cell {selecioneLetras.some(
              (cell) => cell.i === i && cell.j === j
            )
              ? 'selected'
              : ''} {selecionePalavras === puzzle[i][j] ? 'correct' : ''}"
            on:click={() => slecionarLetra(i, j)}
          >
            {coluna}
          </div>
        {/each}
      {/each}
    </div>
  </div>

  <div class="info">
    <p>Escreva a palavra:</p>
    <input type="text" on:input={checagemDeDigito} />
    <p>{palavrasRestantes} palavra(s) restante(s)</p>
    {#if selecionePalavras}
      <div class="palavra">
        Parabéns! Você encontrou: {selecionePalavras}!
      </div>
    {/if}
    {#if todasPalavrasEncontradas()}
      <div class="mensagem-final">
        Parabéns! Você encontrou todas as palavras.
      </div>
    {/if}
  </div>
</main>
<button class='menu' on:click={() => trocarEstadoDoJogo('menu')}>
  Voltar ao Menu
</button>

<style>
  /* Estilos do jogo */

  main {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .container {
    margin-top: 20px;
  }

  .puzzle {
    display: grid;
    grid-template-columns: repeat(7, 40px);
    grid-template-rows: repeat(5, 40px);
    gap: 4px;
  }

  .cell {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: black;
    cursor: pointer;
    color: white;
    font-size: 18px;
    font-weight: bold;
  }

  .selected {
    background-color: red;
  }

  .correct {
    background-color: #70c090;
  }

  .info {
    margin-top: 20px;
    text-align: center;
  }

  .palavra {
    font-size: 20px;
    margin-top: 16px;
    font-weight: bold;
    color: #70c090;
  }

  .mensagem-final {
    font-size: 20px;
    margin-top: 16px;
    font-weight: bold;
    color: #70c090;
  }
</style>
