<script>
  import { v4 as uuidv4 } from "uuid";
  import Card from "./Card.svelte";
  import Button from "./Button.svelte";
  import RatingSelect from "./RatingSelect.svelte";
  import { FeedbackStore } from "../stores";

  let text = "";
  let btnDisabled = true;
  let minCharacterCount = 10;
  let message;
  let rating = 10;
  const handleInput = () => {
    if (text.trim().length <= minCharacterCount) {
      message = `Text must be at least ${minCharacterCount} characters`;
      btnDisabled = true;
    } else {
      message = null;
      btnDisabled = false;
    }
  };

  const handleSelect = (e) => {
    rating = e.detail;
  };

  const handleSubmit = () => {
    if (text.trim().length > minCharacterCount) {
      const newFeedback = {
        id: uuidv4(),
        text: text,
        rating: +rating,
      };

      FeedbackStore.update((currentList) => {
        return [newFeedback, ...currentList];
      });

      text = "";
    }
  };
</script>

<Card>
  <header>
    <h2>How would you rate our service ?</h2>
  </header>
  <!-- Select Rating -->
  <form action="" on:submit|preventDefault={handleSubmit}>
    <RatingSelect on:rating-select={handleSelect} />
    <div class="input-group">
      <input
        type="text"
        bind:value={text}
        on:input={handleInput}
        placeholder="What keeps you coming back ?"
      />
      <Button disabled={btnDisabled} type="submit">Send</Button>
    </div>
    {#if message}
      <div class="message">
        {message}
      </div>
    {/if}
  </form>
</Card>

<style>
  header {
    max-width: 400px;
    margin: auto;
  }

  header h2 {
    font-size: 22px;
    font-weight: 600;
    text-align: center;
  }

  .input-group {
    display: flex;
    flex-direction: row;
    border: 1px solid #ccc;
    padding: 8px 10px;
    border-radius: 8px;
    margin-top: 15px;
  }

  input {
    flex-grow: 2;
    border: none;
    font-size: 16px;
  }

  input:focus {
    outline: none;
  }

  .message {
    padding-top: 10px;
    text-align: center;
    color: rebeccapurple;
  }
</style>
