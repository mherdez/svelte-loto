<script>
  const createNumbers = () =>
    Array(90)
      .fill(null)
      .map((i, index) => index + 1)
      .sort(() => Math.random() - 0.5);

  const cartilla = createNumbers().slice(0, 9);
  const bingo = createNumbers();
  let bolasSacadas = []
  let bolas = [];

  const ciclo = setInterval( () => {
    const ball = +bingo.splice(0, 1);
    bolas = [...bolas, ball];
    if( cartilla.includes(ball) ) bolasSacadas = [...bolasSacadas, ball];
    if( bolasSacadas.length >= 9 ) clearInterval(ciclo)
  },500)
</script>

<main>
  <div class="bingo">
    {#each bolas as bola}
      <span>{bola}</span>
    {/each}
  </div>
  <section>
    {#each cartilla as item}
      <div class={bolas.includes(item) ? 'celda ok' : 'celda'}>{item}</div>
    {/each}
  </section>
</main>

<style>
  .bingo {
    display: grid;
    grid-template-columns: repeat(7, 5rem);
    margin: 1rem;
    gap: .5rem;
    width: 80%;
    position: absolute;
    top: 0;
    left: 0;
  }
  .bingo span {
    font-size: 2rem;
    border: 5px solid yellow;
    border-radius: 50%;
    color: white;
    line-height: 2rem;
    width: 2rem;
    display: inline-block;
    padding: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  section {
    display: grid;
    grid-template-columns: repeat(3, 200px);
  }
  .celda {
    width: 200px;
    height: 200px;
    border: 5px solid yellow;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 6rem;
  }
  .ok {
    background-color: rebeccapurple;
  }
</style>
