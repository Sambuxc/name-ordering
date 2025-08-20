<template>
  <div
      v-if="savedLists.length > 0"
      class="saved-lists"
  >
    <h2>Saved Lists</h2>
    <template
        v-for="list in savedLists"
        :key="list.name"
    >
      <button
          @click="restoreList(list)"
      >
        {{ list.name }}
      </button>
    </template>
  </div>
  <div class="controls-wrapper">
    <div class="controls">
      <button
          :disabled="!isListLoaded"
          @click="clearList"
      >
        Clear All
      </button>
      <button
          :disabled="!isListLoaded"
          @click="saveList"
      >
        Save list
      </button>
      <button
          :disabled="!isListLoaded"
          @click="sortList('alpha')"
      >
        Sort
      </button>
      <button
          :disabled="!isListLoaded"
          @click="sortList('shuffle')"
          aria-label="Shuffle the deck!"
          title="Shuffle the deck!"
      >
        Shuffle
      </button>
    </div>
    <input
        type="text"
        name="input-name"
        @keydown="inputChange($event)"
        v-model="inputVal"
        placeholder="Enter a name"
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
  import { shuffle as _shuffle } from 'lodash-es'
  import { computed, ref } from "vue"

  const inputVal = ref('')

  const items = ref([])
  const savedLists = ref({});
  retrieveLocalStorageLists();

  /**
   * Get all the saved lists from local storage and return them as an array of objects
   * @returns {Array} - Array of objects with the list name and the list
   */
  function retrieveLocalStorageLists() {
    // filter out of local storage all items that start with `list-` and have a length greater than 2 to avoid empty lists
    // such as, `[]`.
    const listNames = Object.keys(localStorage).filter(key => key.startsWith('list-') && localStorage.getItem(key).length > 2)

    savedLists.value = listNames.map(name => {
      const list = localStorage.getItem(name)
      return {
        name: name.replace('list-', ''),
        data: JSON.parse(list)
      }
    })
  }

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

  /**
   * Save the list items to local storage, prompting the user to give the list a name
   * @returns {void}
   */
  function saveList() {
    const list = JSON.stringify(items.value)
    // prompt user with alert to give the list a name
    const listName = prompt('Please enter a name for the list', 'Team Red')
    localStorage.setItem(`list-${listName}`, list)
    retrieveLocalStorageLists();
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

  /**
   * Loads list data by setting the items to the list items.
   * If the list is empty, it will set the items to the list data.
   * @param list
   * @returns {void}
   */
  function restoreList(list) {
    if (!!list) {
      items.value = list.data
    }
  }

  const isListLoaded = computed(() => {
    return items.value.length > 0
  })
</script>

<style scoped>
  .saved-lists {
    margin-left: 5px;
    display: flex;
    flex-flow: column;
    align-items: flex-start;
    gap: 5px;
    position: absolute;
    left: 0;
    top: 0;

    button {
      margin-left: 10px;
    }
  }

  .controls-wrapper {
    margin: 0 auto;
    width: 100%;
    display: flex;
    flex-flow: column;
    align-items: center;
    gap: 5px;

    .controls {
      display: flex;
      gap: 5px;
    }

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
