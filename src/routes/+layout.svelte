<script>
  import "../app.postcss";
  import Header from "./Header.svelte";
  import "./styles.css";
</script>

<div class="bg-[#f5f7f9] flex itmes-center justify-center">
  <div class="w-[1440px] min-h-[100vh]">
    <Header />

    <main>
      <slot />
    </main>

    <footer>
      <p />
    </footer>
  </div>
</div>

<style>
</style>
