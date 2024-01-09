<script>
  import { createEventDispatcher } from "svelte";
  import PollButton from "../shared/PollButton.svelte";
  import PollStore from "../stores/PollStore";
  import axios from "axios";
  import { v4 as uuidv4 } from 'uuid';

  let fields = {
    question: "",
    answerA: "",
    answerB: ""
  };
  let errors = {
    question: "",
    answerA: "",
    answerB: ""
  };
  let valid = false;
  const dispatch = createEventDispatcher();
  const handleSubmit = async() => {
    valid = true;
    // validate questions
    if (fields.question.trim().length < 5) {
      valid = false;
      errors.question = "Question must be at least 5 characters long";
    }else{
        errors.question="";
    }
    // validate answerA
    if (fields.answerA.trim().length < 1) {
      valid = false;
      errors.answerA = "Answer A cannot be empty";
    }else{
        errors.answerA="";
    }
    // validate answerB
     if (fields.answerB.trim().length < 1) {
      valid = false;
      errors.answerB = "Answer B cannot be empty";
    }else{
        errors.answerB="";
    }
    // adding a new poll
    if(valid){
        let poll={...fields, voteA:0, voteB:0, id:uuidv4()}
        // saving data to store
        PollStore.update(currentPoll=>{
          return [poll, ...currentPoll];
        })
        dispatch("add", poll)
        await axios.post(import.meta.env.VITE_API, poll)
    }
  };
</script>

<form
  on:submit|preventDefault={handleSubmit}
  class="w-[400px] mx-auto text-center"
>
  <div class="my-[18px] mx-auto text-left space-y-2">
    <label for="question" class="my-[18px] pb-6">Poll Question:</label>
    <input
      class="w-full px-3 py-2 border rounded-md outline-none"
      type="text"
      id="question"
      bind:value={fields.question}
    />
    <div class="text-xs font-bold text-red-600">{errors.question}</div>
  </div>
  <div class="my-[18px] mx-auto text-left space-y-2">
    <label for="answer-a" class="my-[18px] pb-6">Answer A:</label>
    <input
      class="w-full px-3 py-2 border rounded-md outline-none"
      type="text"
      id="answer-a"
      bind:value={fields.answerA}
    />
    <div class="text-xs font-bold text-red-600">{errors.answerA}</div>
  </div>

  <div class="my-[18px] mx-auto text-left space-y-2">
    <label for="answer-b" class="my-[18px]">Answer B:</label>
    <input
      class="w-full px-3 py-2 border rounded-md outline-none"
      type="text"
      id="answer-b"
      bind:value={fields.answerB}
    />
    <div class="text-xs font-bold text-red-600">{errors.answerB}</div>
  </div>
  <PollButton type="secondary" flat={true}>Add Poll</PollButton>
</form>

<style>
</style>
