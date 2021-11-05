<script>
  import Hole from './Hole.svelte'

  let holesEl, holes

  let lastHole
  let timeUp = false
  let score = 0
  let isDisabled = false
  let value = 6

  $: numberOfHoles = value
  $: holes = holesEl?.children

  const randomTime = (min, max) => Math.round(Math.random() * (max - min) + min)

  function randomHole(holes) {
    const idx = Math.floor(Math.random() * holes.length)
    const hole = holes[idx]

    if (hole === lastHole) return randomHole(holes)

    lastHole = hole

    return hole
  }

  function peep() {
    const time = randomTime(200, 1000)
    const hole = randomHole(holes)

    hole.classList.add('up')

    setTimeout(() => {
      hole.classList.remove('up')
      if (!timeUp) peep()
    }, time)
  }

  function startGame() {
    isDisabled = true
    timeUp = false
    score = 0
    peep()
    setTimeout(() => {
      timeUp = true
      isDisabled = false
    }, 10 * 1000)
  }
</script>

<h1>Whack-a-mole! <span>{score}</span></h1>
<div>
  <input type="number" placeholder="No. of moles" bind:value />
  <button on:click={startGame} disabled={isDisabled}>Start!</button>
</div>

<main bind:this={holesEl}>
  {#each Array(numberOfHoles) as _, index}
    <Hole {index} bind:score />
  {/each}
</main>

<style>
  div {
    display: flex;
    justify-content: center;
  }
  h1 {
    text-align: center;
    font-size: 10rem;
    line-height: 1;
    margin-bottom: 0;
  }

  button,
  input {
    padding: 1rem 1.5rem;
    text-align: center;
    font-size: 2rem;
  }

  main {
    width: 600px;
    height: 400px;
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
  }

  span {
    background: rgba(255, 255, 255, 0.2);
    padding: 0 3rem;
    line-height: 1;
    border-radius: 1rem;
  }
</style>
