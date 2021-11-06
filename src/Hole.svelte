<script>
  export let score = 0

  let holeEl
</script>

<article
  bind:this={holeEl}
  class={score}
  on:click={() => holeEl.classList.remove('up')}
>
  <div on:click={() => score++} />
</article>

<style>
  article {
    flex: 1 0 33.33%;
    overflow: hidden;
    position: relative;
  }

  article:after {
    display: block;
    background: url('/dirt.svg') bottom center no-repeat;
    background-size: contain;
    content: '';
    width: 100%;
    height: 70px;
    position: absolute;
    z-index: 2;
    bottom: -30px;
  }

  div {
    background: url('/mole.svg') bottom center no-repeat;
    background-size: 60%;
    position: absolute;
    top: 100%;
    width: 100%;
    height: 100%;
    transition: all 0.4s;
  }

  article.up div {
    top: 0;
  }
</style>
