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
</script>

<style scoped>
ul {
  list-style: none;
  padding-left: 0;
  width: 789px;
  display: flex;
  flex-wrap: wrap;
}
li {
  margin: 8px 8px 0 0;
  padding: 8px;
  width: fit-content;
  border-radius: 13px;
  outline: 1px solid lightblue;
  background: hsla(160, 100%, 37%, 1);
  color: whitesmoke;
  pointer-events: none;
  text-transform: capitalize;
  transition: 1s all ease-in-out;
}
</style>
