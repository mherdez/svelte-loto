<script>
  import confetti from 'canvas-confetti';

  const createNumbers = () =>
    Array(90)
      .fill(null)
      .map((i, index) => index + 1)
      .sort(() => Math.random() - 0.5);

  let cartilla = createNumbers().slice(0, 9);
  let bingo = createNumbers();
  let bolasSacadas = [];
  let bolas = [];
  let winner = false;

  const iniBingo = () => {
    const ciclo = setInterval(() => {
    const ball = +bingo.splice(0, 1);
    bolas = [ball, ...bolas];
    if (cartilla.includes(ball)) bolasSacadas = [...bolasSacadas, ball];
    if (bolasSacadas.length >= 9) {
      confetti();
      winner = true;
      clearInterval(ciclo);
    }
  }, 1000);
  }

  iniBingo()

  const newGame = () => {
    cartilla = createNumbers().slice(0, 9);
    bingo = createNumbers();
    bolasSacadas = [];
    bolas = [];
    winner = false;
    iniBingo();
  };
</script>

<h1>Bingo</h1>
<main>
  <section>
    {#each cartilla as item}
      <div class="celda"><span class={bolas.includes(item) ? 'ok' : ''}>{item}</span></div>
    {/each}
  </section>
  <div class="bingo">
    {#each bolas as bola, index}
      <span class={index === 0 ? 'first' : ''}>{bola}</span>
    {/each}
  </div>

  {#if winner}
    <div class="winner">
      <p>BINGO!!!</p>
      <div class="newGame" on:click={newGame}>New game</div>
    </div>
  {/if}
</main>

<style>
  h1 {
    text-align: center;;
  }
  main {
    display: flex;
    flex-direction: column;;
    justify-content: center;
    align-items: start;
    padding: 2rem;
  }
  .bingo {
    /* order: 2; */
    display: grid;
    grid-template-columns: repeat(6, 4rem);
    margin: 2rem auto;
  }
  .bingo .first {
    grid-column: 1 / -1;
    border: 10px solid yellow;
    font-size: 3rem;
    inline-size: 3rem;
    height: 3rem;
    padding: 2.5rem;
    text-align: center;
    justify-self: center;
    align-self: center;
    margin-bottom: 1rem;
    background-color: rgb(102, 0, 203);
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
    grid-template-columns: repeat(3, 130px);
    margin: 0 auto;
  }
  .celda {
    height: 130px;
    /* order: 1; */
    border: 3px solid rgb(255, 255, 0);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 3rem;
    text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.736);
  }
  .ok {
    background-color: rgb(73, 16, 131);
    border: 5px solid rgb(255, 255, 0);
    border-radius: 50%;
    width: 60%;
    height: auto;
    aspect-ratio: 1/1;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    box-shadow: 5px 5px 10px 2px black;
    font-size: 2.5rem;
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
    -webkit-text-stroke: 0.2rem white;
    text-shadow: 5px 5px 15px black;
    color: red;
  }
  .newGame {
    font-size: 1.5rem;
    background-color: rebeccapurple;
    padding: 1rem 2rem;
    border: 0.2rem solid white;
    border-radius: 25px;
    text-shadow: 5px 5px 10px black;
    cursor: pointer;
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
    background-color: rgb(102, 0, 203);
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
    border: 3px solid rgb(255, 255, 0);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 5.5rem;
    text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.736);
  }
  .ok {
    background-color: rgb(73, 16, 131);
    border: 5px solid rgb(255, 255, 0);
    border-radius: 50%;
    width: 75%;
    height: auto;
    aspect-ratio: 1/1;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    box-shadow: 5px 5px 10px 2px black;
    font-size: 4.5rem;
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
    -webkit-text-stroke: 0.2rem white;
    text-shadow: 5px 5px 15px black;
    color: red;
  }
  .newGame {
    font-size: 1.5rem;
    background-color: rebeccapurple;
    padding: 1rem 2rem;
    border: 0.2rem solid white;
    border-radius: 25px;
    text-shadow: 5px 5px 10px black;
    cursor: pointer;
  }
}
</style>
