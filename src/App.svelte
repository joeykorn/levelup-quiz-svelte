<script>
  import Quiz from "./Quiz.svelte";
</script>

<style>
  :global(body) {
    background: #524763;
  }

  h1 {
    color: #fff;
    text-align: center;
  }

  div {
    background: #fff;
    border-radius: 15px;
    padding: 40px;
  }
</style>

<h1>Level Up Music</h1>
<div>
  <Quiz />
</div>
