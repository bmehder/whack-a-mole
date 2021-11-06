<script>
  import Hole from './Hole.svelte'

  let holesEl
  let holes = []
  let lastHole
  let numberOfHoles = 6
  let score = 0
  let isTimeUp = false
  let isDisabled = false

  $: holes = holesEl?.children

  const getRandomTime = (min, max) =>
    Math.round(Math.random() * (max - min) + min)

  function getRandomHole(holes) {
    const idx = Math.floor(Math.random() * holes.length)
    const hole = holes[idx]

    if (hole === lastHole) return getRandomHole(holes)

    lastHole = hole

    return hole
  }

  function peep() {
    const randomTime = getRandomTime(200, 1000)
    const randomHole = getRandomHole(holes)

    randomHole.classList.add('up')

    setTimeout(() => {
      randomHole.classList.remove('up')
      if (!isTimeUp) peep()
    }, randomTime)
  }

  function startGame() {
    isDisabled = true
    isTimeUp = false
    score = 0
    peep()
    setTimeout(() => {
      isTimeUp = true
      isDisabled = false
    }, 10 * 1000)
  }
</script>

<h1>Whack-a-mole! <span>{score}</span></h1>
<div>
  <input
    type="number"
    min="2"
    max="32"
    placeholder="No. of moles"
    bind:value={numberOfHoles}
  />
  <button on:click={startGame} disabled={isDisabled}>Start!</button>
</div>

<main bind:this={holesEl}>
  {#each Array(numberOfHoles) as _}
    <Hole bind:score />
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
