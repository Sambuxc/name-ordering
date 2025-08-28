<template>
  <details class="saved-lists-details" open>
    <summary class="saved-lists-header">
      Saved Lists
    </summary>
    <div class="list-wrapper">
      <template v-for="list in savedLists" :key="list.name">
        <button @click="restoreList(list)">
          {{ list.name }}
        </button>
      </template>
    </div>
  </details>
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
  import ListItems from "@/components/ListItems.vue"
  import { shuffle as _shuffle } from 'lodash-es'
  import { ref, computed } from "vue"

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
  /* Mobile-first styles */
  .saved-lists-details {
    position: fixed;
    bottom: 10px;
    left: 0;
    right: 0;
    z-index: 100;
    margin: 0 auto 10px auto;
    display: block;
    width: 95vw;
    max-width: 400px;
    height: fit-content;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.10);
    border: 1px solid #eee;
    border-radius: 8px;
    background: rgba(255, 255, 255, 1);

    .list-wrapper {
      padding-bottom: 10px;
    }
  }

  .saved-lists-header {
    cursor: pointer;
    user-select: none;
    font-size: 1.1rem;
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 12px 16px 8px 16px;
    list-style: none;
    outline: none;
  }

  .saved-lists-details button {
    margin-left: 10px;
    margin-bottom: 4px;
    font-size: 1rem;
    min-width: 80px;
    display: block;
    width: calc(100% - 20px);
    margin-right: 10px;
  }

  .saved-lists-details[open] > summary:before {
    content: "▼ ";
    font-size: 1em;
  }

  .saved-lists-details:not([open]) > summary:before {
    content: "► ";
    font-size: 1em;
  }

  /* Medium devices */
  @media (min-width: 768px) {
    .saved-lists-details {
      max-width: 600px;
      font-size: 1.05rem;
    }

    .saved-lists-header {
      font-size: 1.15rem;
      padding: 14px 20px 10px 20px;
    }
  }

  /* Large devices */
  @media (min-width: 1024px) {
    .saved-lists-details {
      position: absolute;
      left: 0;
      top: 0;
      margin-top: 5px;
      margin-left: 5px;
      max-width: 25vw;
      box-shadow: none;
      z-index: 10;
    }

    .saved-lists-header {
      font-size: 1.2rem;
      padding: 16px 24px 12px 24px;
    }
  }

  .controls-wrapper {
    margin: 0 auto;
    width: 100%;
    display: flex;
    flex-flow: column;
    align-items: center;
    gap: 8px;

    .controls {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    input {
      border-radius: 5px;
    }

    button {
      padding: .7rem 1.1rem;
      width: fit-content;
      background: rgba(255, 255, 255, 0.7);
      font-family: monospace;
      font-size: 1rem;
      min-width: 90px;

      &:active {
        background: rgba(255, 255, 255, 0.8);
      }
    }

    input[type=text] {
      padding: 10px;
      width: 60%;
      background: rgba(255, 255, 255, 0.6);
      font-family: monospace;
      font-size: 1rem;

      &:focus {
        outline: none;
        background: rgba(255, 255, 255, 0.7);
      }
    }
  }
</style>
