<script>
  import { FeedbackStore } from "./../stores.js";
  import FeedbackItem from "./FeedbackItem.svelte";
  import { fade, scale } from "svelte/transition";

</script>

<main>
  {#each $FeedbackStore as fb (fb.id)}
    <!-- content here -->
    <div in:scale out:fade={{ duration: 100 }}>
      <FeedbackItem item={fb}/>
    </div>
  {/each}
</main>
