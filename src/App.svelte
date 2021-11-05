<script>
  import { onMount } from 'svelte'
  import Hole from './Hole.svelte'

  let holes, moles, scoreBoard

  let lastHole
  let timeUp = false
  let score = 0
  const numberOfHoles = 6

  const randomTime = (min, max) => Math.round(Math.random() * (max - min) + min)

  function peep() {
    const time = randomTime(200, 1000)
    const hole = randomHole(holes)

    hole.classList.add('up')

    setTimeout(() => {
      hole.classList.remove('up')
      if (!timeUp) peep()
    }, time)
  }

  function randomHole(holes) {
    const idx = Math.floor(Math.random() * holes.length)
    const hole = holes[idx]

    if (hole === lastHole) return randomHole(holes)

    lastHole = hole

    return hole
  }

  function startGame() {
    timeUp = false
    score = 0
    peep()
    setTimeout(() => (timeUp = true), 10 * 1000)
  }

  onMount(() => {
    holes = document.querySelectorAll('.hole')
    moles = document.querySelectorAll('.mole')
  })
</script>

<h1>Whack-a-mole! <span bind:this={scoreBoard}>{score}</span></h1>
<button on:click={startGame}>Start!</button>

<main bind:this={holes}>
  {#each Array(numberOfHoles) as _, index}
    <Hole {index} bind:score />
  {/each}
</main>

<style>
  :global(html) {
    box-sizing: border-box;
    background: #ffc600;
    font-size: 10px;
  }

  :global(*, *:before, *:after) {
    box-sizing: inherit;
  }

  :global(body) {
    padding: 0;
    margin: 0;
    font-family: 'Amatic SC', cursive;
  }

  h1 {
    text-align: center;
    font-size: 10rem;
    line-height: 1;
    margin-bottom: 0;
  }

  button {
    display: block;
    margin: auto;
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
