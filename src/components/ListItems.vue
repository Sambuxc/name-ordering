<template>
  <TransitionGroup tag="ul" name="fade" class="container">
    <li
        v-for="(item, index) in props.items"
        :key="item"
        class="item"
    >
      {{ item }}
      <button class="remove" @click="$emit('remove', index)">x</button>
    </li>
  </TransitionGroup>
</template>

<script setup>
const props = defineProps({
  items: {
    type: Array
  }
})
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
  overflow: hidden;
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
  cursor: default;

  &:hover button.remove {
    left: 0;
    opacity: 1;
  }
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

button.remove {
  position: absolute;
  appearance: none;
  background: none;
  border-width: 1px;
  position: relative;
  left: 40px;
  opacity: 0;
  transition: .3s all ease;

  &:hover {
    border: 1px solid red;
    cursor: pointer;
  }
}
</style>
