<script>
  import { fade, scale } from "svelte/transition";
  import PollStore from "../stores/PollStore";
  import PollDetail from "./PollDetail.svelte";
  import { flip } from "svelte/animate";
  import axios from 'axios';
  import { isEmpty } from 'underscore'

  const fetchingPolls=async()=>{
    await axios.get(import.meta.env.VITE_API).then((response)=>PollStore.set(response.data))
  }
 
  $:if(isEmpty($PollStore)){
    fetchingPolls()
  }
</script>

<div class="grid grid-cols-2 gap-5">
  {#each $PollStore as poll (poll.id)}
    <div in:fade out:scale|local animate:flip={{duration:800}}>
      <PollDetail {poll} on:vote />
    </div>
  {/each}
</div>
