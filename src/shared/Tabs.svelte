<script>
  import { createEventDispatcher } from "svelte";

  export let tabs;
  export let activeTab;
  const dispatch = createEventDispatcher();
</script>

<div class="mb-10">
  <ul class="flex justify-center p-0 gap-4">
    {#each tabs as tab, i}
      <li on:click={()=>dispatch('tabChange',tab)} class="text-[18px] text-gray-500 cursor-pointer">
        <div
          class={activeTab === tab
            ? `text-red-500 border-b-2 border-b-red-500 pb-2`
            : ``}
        >
          {tab}
        </div>
      </li>
    {/each}
  </ul>
</div>
