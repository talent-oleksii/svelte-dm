<script>
  import { Tabs, Tab, TabContent } from "carbon-components-svelte";
  import "carbon-components-svelte/css/all.css";
  import logoims from "$lib/images/logo_ims.svg";
  import logo from "$lib/images/logo.png";
</script>

<header>
  <div class="px-[50px] py-[35px] bg-[#F5F7F9] flex justify-between">
    <div class="w-3/12 flex items-center">
      <a href="/">
        <img src={logoims} alt="SvelteKit" width="224px" height="63px" />
      </a>
    </div>
    <div class="w-6/12 text-center">
      <h4>Data Loader Config Management</h4>
      <div class="w-[32rem] m-auto pt-5">
        <Tabs autoWidth>
          <Tab label="Inputs" />
          <Tab label="Tables" />
          <Tab label="Scheduling" />
          <Tab label="Reporting" />
          <Tab label="Data Dictionary" />
          <!-- <svelte:fragment slot="content">
          <TabContent>Content 1</TabContent>
          <TabContent>Content 2</TabContent>
          <TabContent>Content 3</TabContent>
        </svelte:fragment> -->
        </Tabs>
      </div>
    </div>
    <div class="w-3/12 flex items-center relative">
      <img src={logo} alt="Logo" class="h-12 absolute right-0" />
    </div>
  </div>
</header>

<style>
</style>
