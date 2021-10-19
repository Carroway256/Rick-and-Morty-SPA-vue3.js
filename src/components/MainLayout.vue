<template>
  <div class="top">
    <img class="logo" src="./rick-and-morty-logo2.png" />
  </div>
  <div class="tabsContainer">
    <p v-on:click="showAll" :class="data.favShown ? 'noactive' : 'active'">
      All Characters
    </p>
    <p v-on:click="showFav" :class="data.favShown ? 'active' : 'noactive'">
      Favorites
    </p>
  </div>
  <div v-if="!data.favShown" >
  <CharacterList v-bind:favShown="data.favShown"/>
  </div>
  <div v-if="data.favShown">
    in progress
  </div>
</template>
<script lang="ts">

import { reactive } from "@vue/reactivity";
import { defineComponent, provide, ref } from "vue";
import CharacterList from "./CharacterList.vue"
export default defineComponent({
  name: "MainLayout",
  components:{
    CharacterList
  },
  setup() {
    const favShown=ref('favShown')
    const data = reactive({
      
      favShown: false as Boolean,
    });
    provide('favShown',favShown)
    function showFav() {
      data.favShown = true;
    }
    function showAll() {
      data.favShown = false;
    }
    return { data: data, showFav: showFav, showAll: showAll };
  },
});
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");
.top {
  border-bottom: 1px solid lightgrey;
  height: 120px;
  margin-left: 0px;
  margin-top: 50px;
}
p {
  font-family: "Poppins";
  font-size: 16px;
  font-style: normal;
  font-weight: 500;
  line-height: 24px;
  letter-spacing: 0px;
  float: left;
  padding: 30px;
}
.tabsContainer {
  padding-left: 10vw;
  overflow: hidden;
}

.active {
  color: #11b0c8;
  text-decoration: underline;
  text-underline-offset: 50%;
  text-decoration-style: solid;
}

.logo {
  min-width: 200px;
  margin-top: -10px;
  float: left;
  margin-left: 50px;
  width: 20%;
  height: auto;
}
</style>
