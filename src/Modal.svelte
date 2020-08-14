<script>
  import { fly, fade } from "svelte/transition";
  import { createEventDispatcher } from "svelte";
  let w;
  const dispatch = createEventDispatcher();
</script>

<style lang="scss">
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.8);

    .modal {
      background: #fff;
      padding: 20px;
      border-radius: 15px;
    }
  }
</style>

<div class="modal-bg" transition:fade bind:clientWidth={w}>
  <div class="modal" transition:fly={{ y: -100 }}>
    <button
      on:click={() => {
        dispatch('close');
      }}>
      Close
    </button>
    <!-- <slot>optional fallback</slot> -->
    <!-- <slot name="modal" /> -->
    <slot />
  </div>
</div>
