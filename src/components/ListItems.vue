<template>
  <TransitionGroup tag="ul" name="fade" class="container">
    <li
        v-for="(item, index) in props.items"
        :key="item"
        class="item"
    >
      <div class="fancy-border"></div>
      <div class="content-wrapper">
        {{ item }}
        <button class="remove" @click="$emit('remove', index)">x</button>
      </div>
    </li>
  </TransitionGroup>
</template>

<script setup>
  const props = defineProps({
    items: {
      type: Array
    }
  })

  defineEmits(['remove'])
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
    //outline: 1px solid var(--vt-c-white-soft);
    background: hsla(160, 100%, 37%, 1);
    color: var(--vt-c-black);
    text-transform: capitalize;
    font-family: monospace;
    transition: 1s all ease-in-out;
    cursor: default;

    .content-wrapper {
      position: relative;
      overflow: hidden;
    }

    &:hover button.remove {
      left: 0;
      opacity: 1;
    }

    &:hover .fancy-border {
      opacity: 1;
    }

    .fancy-border {
      opacity: 0;
      position: absolute;
      top: 0;
      left: 0;
      content: '';
      border: 2px solid greenyellow;
      width: 100%;
      height: 100%;
      padding: 8px;
      border-radius: inherit;
      animation: animate 2s linear infinite;
      transition: .5s opacity ease;
    }

    @keyframes animate {
      50% {
        filter: hue-rotate(350deg);
      }

    }
  }

  ul:has(li:hover) li:not(:hover) {
    opacity: 0.2;
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
    border: 0;
    position: relative;
    left: 40px;
    background: teal;
    color: var(--color-text);
    opacity: 0;
    transition: .3s all ease;
    cursor: pointer;
  }

</style>
