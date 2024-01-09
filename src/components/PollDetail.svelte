<script>
  import { createEventDispatcher } from "svelte";
  import Card from "../shared/Card.svelte";
  import PollStore from "../stores/PollStore";
  import PollButton from "../shared/PollButton.svelte";
  import { tweened } from "svelte/motion";
  import axios from "axios";

  export let poll;
  const dispatch = createEventDispatcher();
  // reactive values
  $: totalVotes = poll.voteA + poll.voteB;
  $: percentA = Math.floor((100 / totalVotes) * poll.voteA) || 0;
  $: percentB = Math.floor((100 / totalVotes) * poll.voteB) || 0;
  //  tweened percentages
  const tweenedA = tweened(0);
  const tweenedB = tweened(0);
  $: tweenedA.set(percentA);
  $: tweenedB.set(percentB);

  //   handling votes
  const handleVote = async (option, id) => {
    await PollStore.update((currentPoll) => {
      let copiedPoll = [...currentPoll];
      let upvoted = copiedPoll.find((poll) => poll.id == id);
      if (option === "a") {
        {
          upvoted.voteA++;
        }
      }
      if (option === "b") {
        {
          upvoted.voteB++;
        }
      }
      axios.put(import.meta.env.VITE_API + `/${id}`, null, {
        params: { voteA: upvoted.voteA, voteB: upvoted.voteB },
      });
      return copiedPoll;
    });
  };

  // deleting poll
  const handleDelete = async (id) => {
    await PollStore.update((currentPoll) => {
      axios.delete(import.meta.env.VITE_API + `/${id}`);
      return currentPoll.filter((poll) => poll.id != id);
    });
  };
</script>

<Card>
  <h3 class="mx-auto text-xl font-bold text-gray-500">{poll.question}</h3>
  <p class="mt-[6px] text-[14px] mb-[30px] text-gray-400">
    Total votes {totalVotes}
  </p>
  <div
    class=" cursor-pointer my-[10px] relative bg-gray-100 hover:opacity-60"
    on:click={() => handleVote("a", poll.id)}
  >
    <div
      style="width:{$tweenedA}%"
      class={`box-border border-l-4  border-l-green-500 absolute h-full bg-green-600 bg-opacity-20`}
    ></div>
    <span class="inline-block py-[10px] px-5"
      >{poll.answerA} ({poll.voteA})</span
    >
  </div>
  <div
    class=" cursor-pointer my-[10px] relative bg-gray-100 hover:opacity-60"
    on:click={() => handleVote("b", poll.id)}
  >
    <div
      style="width:{$tweenedB}%"
      class={`box-border  border-l-4 border-l-red-500 absolute h-full bg-red-500 bg-opacity-20`}
    ></div>
    <span class="inline-block py-[10px] px-5"
      >{poll.answerB} ({poll.voteB})</span
    >
  </div>
  <div class="mt-4">
    <PollButton flat={true} on:click={() => handleDelete(poll.id)}>
      Delete
    </PollButton>
  </div>
</Card>
