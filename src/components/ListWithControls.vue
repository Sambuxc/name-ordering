<template>
  <div class="controls-wrapper">
    <div class="">
      <button @click="clearList">Clear All</button>
      <button @click="sortList('alpha')">Sort</button>
      <button @click="sortList('shuffle')" aria-label="Shuffle the deck!"
              title="Shuffle the deck!">Shuffle
      </button>
    </div>
    <input
        type="text"
        name="input-name"
        @keydown="inputChange($event)"
        v-model:="inputVal"
    />
  </div>

  <ListItems
      v-if="items.length > 0"
      :items="items"
      @remove="remove"
  />
</template>

<script setup>
// TODO: Change sort button to select dropdown with 2 options; Alphabetical, Alphabet Reversed
import ListItems from "@/components/ListItems.vue"
import {shuffle as _shuffle} from 'lodash-es'
import {ref} from "vue"

const inputVal = ref('')

const items = ref([])


function inputChange(e) {
  const keyCode = e.code
  switch (keyCode) {
    case 'Enter':
      if (!!inputVal.value) {
        items.value.push(inputVal.value.toLowerCase())
        // items.value.reverse()
        inputVal.value = ''
      }
      break;
  }
}

function clearList() {
  items.value = []
}

function sortList(type) {
  switch (type) {
    case 'alpha':
      items.value.sort()
      break;

    case 'shuffle':
      items.value = _shuffle(items.value)
      break;
  }
}

function remove(index) {
  items.value.splice(index, 1)
}
</script>

<style scoped>

.controls-wrapper {
  margin: 0 auto;
  width: 100%;
  display: flex;
  flex-flow: column;
  align-items: center;
  gap: 5px;

  input {
    border-radius: 5px;
  }

  button {
    padding: .5rem;
    width: fit-content;
    background: rgba(255, 255, 255, 0.7);
    font-family: monospace;
    font-size: .7rem;

    &:active {
      background: rgba(255, 255, 255, 0.8);
    }
  }

  input[type=text] {
    padding: 8px;
    width: 60%;
    background: rgba(255, 255, 255, 0.6);
    font-family: monospace;

    &:focus {
      outline: none;
      background: rgba(255, 255, 255, 0.7);
    }
  }
}
</style>
