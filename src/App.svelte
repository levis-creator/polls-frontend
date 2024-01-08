<script>
  import Footer from "./components/Footer.svelte";
  import Header from "./components/Header.svelte";
  import PollForm from "./components/PollForm.svelte";
  import PollList from "./components/PollList.svelte";
  import Tabs from "./shared/Tabs.svelte";
  let tabs = ["Current polls", "Add New Poll"];
  let activeTab = "Current polls";

  const tabChange = (e) => {
    activeTab = e.detail;
  };

  const handleAdd = (e) => {
    activeTab = "Current polls";
    
  };
</script>

<Header />
<main class="max-w-[960px] my-10 mx-auto">
  <Tabs {activeTab} {tabs} on:tabChange={tabChange} />
  {#if activeTab === tabs[0]}
    <PollList />
  {:else if activeTab === tabs[1]}
    <PollForm on:add={handleAdd} />
  {/if}
</main>
<Footer />

<style>
</style>
