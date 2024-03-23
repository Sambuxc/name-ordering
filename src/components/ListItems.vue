<template>
  <TransitionGroup tag="ul" name="fade" class="container">
    <li
        v-for="(item, index) in props.items"
        :key="item"
        class="item"
    >
      {{ item }}
<!--      <button @click="$emit('remove')">x</button>-->
    </li>
  </TransitionGroup>
</template>

<script setup>
const props = defineProps({
  items: {
    type: Array
  }
})
// TODO: on hover li, truncate text and animate X button to remove it

</script>

<style scoped>
.container {
  position: relative;
  margin-top: 1rem;
  margin-left: auto;
  margin-right: auto;
  padding: 0;
  list-style: none;
  width: 60%;
}

li {
  position: relative;
  margin: 8px 8px 0 0;
  padding: 8px;
  width: fit-content;
  border-radius: 13px;
  outline: 1px solid var(--vt-c-white-soft);
  background: hsla(160, 100%, 37%, 1);
  color: var(--vt-c-black);
  text-transform: capitalize;
  font-family: monospace;
  transition: 1s all ease-in-out;
}


/* 1. declare transition */
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. declare enter from and leave to state */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}

/* 3. ensure leaving items are taken out of layout flow so that moving
      animations can be calculated correctly. */
.fade-leave-active {
  position: absolute;
}
</style>
