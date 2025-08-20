<script setup>
defineProps({
  heading: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ heading }}</h1>
    <h3>Give it a try and let us know what you think :)</h3>
  </div>
</template>

<style scoped>
.greetings {
  margin-bottom: 1rem;
}
h1 {
  font-weight: 500;
  font-size: 180%;
  position: relative;
  top: -10px;

  @media (min-width: 1024px) {
    font-size: 2.6rem;
  }
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}
</style>
