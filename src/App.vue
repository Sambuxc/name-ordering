<template>
  <header>
    <div class="wrapper">
      <HelloWorld msg="Name Ordering Has Never Been Cooler"/>
    </div>
  </header>

  <main>
    <div class="input-wrapper">
      <div>
        <input type="button" value="Clear All" @click="clearList">
        <input type="button" value="Sort" @click="sortList('alpha')">
      </div>
      <input
          type="text"
          name="input-name"
          @keydown="inputChange($event)"
          v-model:="nameVal"
      />
    </div>

    <NameList
        v-if="nameArr.length > 0"
        :list="nameArr"
    />
  </main>
</template>

<script setup>
import {ref} from "vue";
import HelloWorld from './components/HelloWorld.vue'
import NameList from "@/components/NameList.vue";

const nameVal = ref('')

const nameArr = ref([]);

function inputChange(e) {
  const keyCode = e.code
  switch (keyCode) {
    case 'Enter':
      if (!!nameVal.value) {
        nameArr.value.push(nameVal.value.toLowerCase())
        nameVal.value = ''
      }
      break;
  }
}

function clearList() {
  nameArr.value = [];
}

function sortList(type) {
  switch (type) {
    case 'alpha':
      nameArr.value.sort()
      break;
  }
}
</script>

<style scoped>
header {
  line-height: 1.5;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    flex-direction: column;
    place-items: center;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  place-content: center;
  width: 100%;
}

.input-wrapper {
  margin: 0 auto;
  width: 100%;
  display: flex;
  flex-flow: column;
  align-items: center;
  gap: 5px;

  input {
    border-radius: 5px;
  }

  input[type=button] {
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
