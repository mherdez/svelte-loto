<script>
  import confetti from 'canvas-confetti'

  const createNumbers = () =>
    Array(90)
      .fill(null)
      .map((i, index) => index + 1)
      .sort(() => Math.random() - 0.5);

  let cartilla = createNumbers().slice(0, 25);
  let bingo = createNumbers();
  let bolasSacadas = [];
  let bolas = [];
  let winner = false;
  let bolasWin = []

  const combos = [
    [0,1,2,3,4],
    [5,6,7,8,9],
    [10,11,12,13,14],
    [15,16,17,18,19],
    [20,21,22,23,24],
    [0,5,10,15,20],
    [1,6,11,16,21],
    [2,7,12,17,22],
    [3,8,13,18,23],
    [4,9,14,19,24],
    [0,6,12,18,24],
    [4,8,12,16,20],
  ]

  const iniBingo = () => {
    const ciclo = setInterval(() => {
      const ball = +bingo.splice(0, 1);
      bolas = [ball, ...bolas];
      if (cartilla.includes(ball)) bolasSacadas = [...bolasSacadas, ball];
      if (isWinner()) {
        confetti();
        celdaWin();
        winner = true;
        clearInterval(ciclo);
      }
    }, 100);
  };

  iniBingo();

  // TODO: Hacer todas las combinaciones ganadoras
  const isWinner = () => combos.some( (jugada) => jugada.every( check => bolasSacadas.includes(cartilla[check])));

  const celdaWin = () => {
    combos.forEach( jugada => {
      if (jugada.every( check => bolasSacadas.includes(cartilla[check])))
      {
        bolasWin = [...jugada.map( i => cartilla[i])]
      }
    })
  }

  const newGame = () => {
    cartilla = createNumbers().slice(0, 25);
    bingo = createNumbers();
    bolasSacadas = [];
    bolas = [];
    winner = false;
    iniBingo();
    bolasWin = []
  };
</script>

<h1>Lotería Campechana</h1>
<main>
  <section>
    {#each cartilla as item}
      <div class={bolasWin.includes(item) ? 'celda celdaWin' : 'celda'}>
        <div class={bolas.includes(item) ? 'ok' : ''}></div>
        <img src="./assets/{item}.png" alt="img{item}" />
      </div>
    {/each}
  </section>
  <div class="bingo">
    {#each bolas as bola, index}
      <span class={index === 0 ? 'first' : ''}>{bola}</span>
    {/each}

    {#if winner}
    <div class="winner">
      <p>LOTERÍA!!!</p>
      <div class="newGame" on:click={newGame} on:keydown={newGame}>New game</div>
    </div>
    {/if}
  </div>
</main>

<style>
  h1 {
    text-align: center;
    letter-spacing: .08rem;
  }
  main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: start;
    padding: 0.5rem 0;
  }
  .bingo {
    /* order: 2; */
    display: grid;
    grid-template-columns: repeat(6, 3.5rem);
    margin: 2rem auto;
    position: relative;
    width: 100%;
    justify-content: center;
  }
  .bingo .first {
    grid-column: 1 / -1;
    border: 10px solid rgb(255, 200, 0);
    font-size: 3.3rem;
    inline-size: 2rem;
    height: 2rem;
    padding: 2.5rem;
    text-align: center;
    justify-self: center;
    align-self: center;
    margin-bottom: 1rem;
    background-color: rgb(157, 90, 1);
    font-weight: bold;
    text-shadow: 5px 5px 10px black;
  }
  .bingo span {
    border: 3px solid rgba(255, 255, 0, 0.766);
    border-radius: 50%;
    color: white;
    font-size: 1rem;
    line-height: 1rem;
    width: 1rem;
    display: inline-block;
    padding: 0.8rem;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0.5rem 0;
  }
  section {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    margin: 0 auto;
    gap: .3rem;
    padding: 1rem;
  }
  .celda {
    height: 3.5rem;
    /* order: 1; */
    /* border: 3px solid rgb(255, 255, 0); */
    border: 3px solid rgba(0, 0, 0, 0.736);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 3rem;
    text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.736);
    position: relative;
    border-radius: 10px;
  }
  .celdaWin {
    border: 3px solid rgb(255, 200, 0);
  }
  .celda img {
    height: 100%;
    width: 100%;
  }
  .ok {
    position: absolute;
    /* background-color: rgb(73, 16, 131);
    border: 5px solid rgb(255, 255, 0); */
    /* border-radius: 50%; */
    width: 70%;
    height: auto;
    aspect-ratio: 1/1;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    /* box-shadow: 5px 5px 10px 2px black; */
    /* font-size: 2.5rem; */
    background-image: url('/assets/frijol.png');
    background-position: center;
    background-size: cover;
  }
  .winner {
    width: 100%;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.8);
    position: absolute;
    top: -3%;
    display: flex;
    justify-content: flex-star;
    align-items: center;
    flex-direction: column;
  }
  .winner p {
    font-size: 2.8rem;
    letter-spacing: 0.3rem;
    font-weight: bolder;
    font-family: Arial, Helvetica, sans-serif;
    -webkit-text-stroke: 0.1rem black;
    text-shadow: 5px 5px 15px black;
    /* color: red; */
    color: rgb(197, 112, 1);
  }
  .newGame {
    font-size: 0.8rem;
    background-color: rgb(154, 87, 0);
    padding: 0.8rem 1.5rem;
    border: 0.2rem solid white;
    border-radius: 25px;
    text-shadow: 5px 5px 10px black;
    cursor: pointer;
    margin-top: 1rem;
  }
  @media (min-width: 821px) {
    main {
      display: flex;
      justify-content: center;
      align-items: start;
      padding: 2rem;
      flex-direction: row-reverse;
    }
    .bingo {
      /* order: 1; */
      display: grid;
      grid-template-columns: repeat(6, 5rem);
      margin: 0.5rem;
    }
    .bingo .first {
      grid-column: 1 / -1;
      border: 10px solid yellow;
      font-size: 6rem;
      inline-size: 6rem;
      height: 6rem;
      padding: 2.5rem;
      text-align: center;
      justify-self: center;
      align-self: center;
      margin-bottom: 1rem;
      /* background-color: rgb(102, 0, 203); */
      font-weight: bold;
      text-shadow: 5px 5px 10px black;
    }
    .bingo span {
      border: 3px solid rgba(255, 255, 0, 0.766);
      border-radius: 50%;
      color: white;
      font-size: 1.5rem;
      line-height: 1.5rem;
      width: 1.5rem;
      display: inline-block;
      padding: 0.8rem;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0.5rem;
    }
    section {
      display: grid;
      grid-template-columns: repeat(3, 200px);
      flex: 1;
      margin: 2rem 5rem;
    }
    .celda {
      /* order: 2; */
      width: 200px;
      height: 200px;
      border: 3px solid black;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 5.5rem;
      text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.736);
    }
    .ok {
      /* background-color: rgb(73, 16, 131); */
      /* border: 5px solid rgb(255, 255, 0); */
      border-radius: 50%;
      width: 75%;
      height: auto;
      aspect-ratio: 1/1;
      display: flex;
      justify-content: center;
      align-items: center;
      /* font-weight: bold; */
      /* box-shadow: 5px 5px 10px 2px black; */
      /* font-size: 4.5rem; */
    }
    .winner {
      width: 100%;
      height: 100vh;
      background-color: rgba(0, 0, 0, 0.8);
      position: absolute;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }
    .winner p {
      font-size: 10rem;
      letter-spacing: 1rem;
      font-weight: bolder;
      font-family: Arial, Helvetica, sans-serif;
      -webkit-text-stroke: 0.2rem black;
      text-shadow: 5px 5px 15px black;
      /* color: red; */
    }
    .newGame {
      font-size: 1.5rem;
      /* background-color: rebeccapurple; */
      padding: 1rem 2rem;
      border: 0.2rem solid white;
      border-radius: 25px;
      text-shadow: 5px 5px 10px black;
      cursor: pointer;
    }
  }
</style>
