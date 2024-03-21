<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <div class="input-wrapper">
      <input
        type="text"
        name="input-name"
        @keydown="inputChange($event)"
        v-model:="nameVal"
      />
      <input type="button" value="Clear All" @click="clearList">
      <input type="button" value="Sort" @click="sortList('alpha')">
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

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    flex-direction: column;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
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
}

.input-wrapper {
  margin: 0 auto;
}
</style>
