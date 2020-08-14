<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Question from "./Question.svelte";
  import Modal from "./Modal.svelte";
  import { score } from "./store.js";
  let activeQuestion = 0;
  let quiz = getQuiz();
  let isModalOpen = false;

  onMount(() => {
    console.log("mounted");
  });

  beforeUpdate(() => {
    console.log("before update");
  });

  afterUpdate(() => {
    console.log("after update");
  });

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=12&type=multiple"
    );
    const quiz = await res.json();
    return quiz;
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function resetQuiz() {
    isModalOpen = false;
    score.set(0);
    activeQuestion = 0;
    quiz = getQuiz();
  }

  // Reactive Statement
  $: if ($score > 0) {
    isModalOpen = true;
  }

  // Reactive Declaration
  $: questionNumber = activeQuestion + 1;
</script>

<style>
  .fade-wrapper {
    position: absolute;
  }
  .container {
    min-height: 500px;
  }
</style>

<div>
  <button on:click={resetQuiz}>Start New Quiz</button>

  <h3>My score: {$score}</h3>
  <h4>Question #{questionNumber}</h4>

  <div class="container">
    {#await quiz}
      Loading...
    {:then data}
      {#each data.results as question, index}
        {#if index === activeQuestion}
          <!-- https://github.com/sveltejs/svelte/issues/3202 -->
          <div
            in:fly={{ x: 100 }}
            out:fly|local={{ x: -200 }}
            class="fade-wrapper">
            <Question {nextQuestion} {question} />
          </div>
        {/if}
      {/each}
    {/await}
  </div>

</div>

{#if isModalOpen}
  <div class="modal-bg">
    <Modal on:close={resetQuiz}>
      <h2>You won!</h2>
      <p>Congratulations</p>
      <button on:click={resetQuiz}>Start Over</button>
    </Modal>
  </div>
{/if}
