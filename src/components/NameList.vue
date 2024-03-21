<template>
  <ul class="name-list">
    <transition-group>
      <li v-for="name in props.list">
        {{ name }}
      </li>
    </transition-group>
  </ul>
</template>

<script setup>

const props = defineProps({
  list: { type: Array }
})
// TODO: read up transition-group -> https://vuejs.org/guide/built-ins/transition-group.html#transitiongroup
// TODO: on hover li, truncate text and animate X button to remove it
</script>

<style scoped>
ul {
  margin-top: 1rem;
  list-style: none;
  padding-left: 0;
  max-width: 789px;
  display: flex;
  flex-wrap: wrap;
}
li {
  margin: 8px 8px 0 0;
  padding: 8px;
  width: fit-content;
  border-radius: 13px;
  outline: 1px solid var(--vt-c-white-soft);
  background: hsla(160, 100%, 37%, 1);
  color: var(--vt-c-black);
  pointer-events: none;
  text-transform: capitalize;
  font-family: monospace;
  transition: 1s all ease-in-out;
}
</style>
