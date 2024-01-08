<script>
  import { fade, scale } from "svelte/transition";
  import PollStore from "../stores/PollStore";
  import PollDetail from "./PollDetail.svelte";
  import { flip } from "svelte/animate";
</script>

<div class="grid grid-cols-2 gap-5">
  {#each $PollStore as poll (poll.id)}
    <div in:fade out:scale|local animate:flip={{duration:800}}>
      <PollDetail {poll} on:vote />
    </div>
  {/each}
</div>
