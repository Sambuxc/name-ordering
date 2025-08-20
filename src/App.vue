<template>
  <main>
    <ListWithControls/>
  </main>
</template>

<script setup>
  import ListWithControls from "@/components/ListWithControls.vue"
</script>

<style scoped>
  main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    place-content: center;
    width: 100%;
  }
</style>
