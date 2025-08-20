<template>
  <header>
    <div class="wrapper">
      <NamePicker heading="Name Ordering Has Never Been Cooler"/>
    </div>
  </header>

  <main>
    <ListWithControls/>
  </main>
</template>

<script setup>
  import NamePicker from './components/NamePicker.vue'
  import ListWithControls from "@/components/ListWithControls.vue";
</script>

<style scoped>
  header {
    line-height: 1.5;
  }

  @media (min-width: 1024px) {
    header {
      display: flex;
      flex-direction: column;
      place-items: center;
    }

    header .wrapper {
      display: flex;
      place-items: flex-start;
      flex-wrap: wrap;
    }
  }

  main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    place-content: center;
    width: 100%;
  }


</style>
