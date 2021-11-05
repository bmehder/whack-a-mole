<script>
  export let index = 0
  export let score = 0
</script>

<div class="hole hole{index}">
  <div class="mole" on:click={() => score++} />
</div>

<style>
  .hole {
    flex: 1 0 33.33%;
    overflow: hidden;
    position: relative;
  }

  .hole:after {
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

  .mole {
    background: url('/mole.svg') bottom center no-repeat;
    background-size: 60%;
    position: absolute;
    top: 100%;
    width: 100%;
    height: 100%;
    transition: all 0.4s;
  }

  .hole.up .mole {
    top: 0;
  }
</style>
