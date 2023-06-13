<script>
  import { bubble } from "svelte/internal";
  import { trocarEstadoDoJogo } from "./Estado.js";
  let puzzle = [
    ["B", "O", "L", "A", "I", "A", "X"],
    ["R", "A", "Q", "U", "E", "T", "E"],
    ["M", "E", "S", "A", "F", "G", "S"],
    ["J", "O", "G", "A", "D", "O", "R"],
    ["P", "A", "R", "T", "I", "D", "A", "D"],
  ];
  let palavras = ["bola", "raquete", "mesa", "jogador", "partida"];

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

    if (palavras.includes(palavra)) {
      selecionePalavras = palavra;
      selecioneLetras = [];
    }
  }

  function checagemDeDigito(digito) {
    const palavraDigitada = digito.target.value.toLowerCase();

    if (palavras.includes(palavraDigitada)) {
      selecionePalavras = palavraDigitada;
      selecioneLetras = [];
    } else {
      selecioneLetras = [];
    }
  }
</script>

<main>
  <h2>Jogo de Caça-Palavras</h2>
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
  <p>Escreva a palavra:</p>
  <input type="text" on:input={checagemDeDigito} />
  {#if selecionePalavras}
    <div class="palavra">
      parabéns você achou: {selecionePalavras} !
    </div>
  {/if}
</main>
<button class='menu' on:click={() => trocarEstadoDoJogo('menu')}>
	Voltar ao Menu
</button>

<style>
  .puzzle {
    display: grid;
    grid-template-columns: repeat(6, 50px);
    grid-template-rows: repeat(6, 50px);
    gap: 4px;
  }

  .cell {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: black;
    cursor: pointer;
  }

  .selected {
    background-color: red;
  }

  .correct {
    background-color: #70c090;
  }

  .palavra {
    font-size: 20px;
    margin-top: 16px;
    font-weight: bold;
    color: #70c090;
  }
</style>
