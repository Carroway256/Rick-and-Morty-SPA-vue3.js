<template>
<div id="app">
        <div class="top">
            <img class="logo" src="./rick-and-morty-logo2.png"/>
            
            <div class="inner-addon right-addon">
                <span v-on:click="fetchSingleRick(character)" class="material-icons glyphicon glyphicon-search md-36">search</span>
                <input  v-model="data.character" v-on:input="fetchSingleRick(data.character)" type="text" class="form-control" placeholder="Start typing to search" />
              </div>
        </div>
        <div class="tabsContainer">
            <p v-on:click="showAll" :class="data.favShown ? 'noactive' : 'active'" >All Characters</p>
            <p v-on:click="showFav" :class="data.favShown ? 'active' : 'noactive'">Favorites</p>
        </div>     
        <div >
        <div v-if="data.favShown" >
            <table style="width:100%">
                <tr class="charsAtributes"><th style="text-align:center">Photo</th><th>Character ID</th><th>Name</th><th>Gender</th><th>Species</th><th>Last Episode</th><th>Add To Favorites</th></tr> 
    
                <tr class="singleCharacter" v-for="(info) in data.fav" :key="info.id">
                    <td ><img class="charImage" v-bind:src="info.image"></td>
                    <td>{{info.id}}</td>
                    <td>{{info.name}}</td>
                    <td>{{info.gender}}</td>
                    <td>{{info.species}}</td>
                    <td>{{info.episode}}</td>
                    <td><span class="material-icons md-48 favoriteClicked" >
                        star_rate 
                        </span></td>
                </tr> 
                </table>    
            </div>
        </div>
        <div v-if="!data.favShown">
            <table style="width:100%">
            <tr class="charsAtributes"><th style="text-align:center">Photo</th><th>Character ID</th><th>Name</th><th>Gender</th><th>Species</th><th>Last Episode</th><th>Add To Favorites</th></tr> 
            <tr class="singleCharacter" v-for="(info) in data.infos" :key="info.id">
                <td ><img class="charImage" v-bind:src="info.image"></td>
                <td>{{info.id}}</td>
                <td>{{info.name}}</td>     
                <td><span v-if="info.gender==='Male'" class="material-icons" >male</span>
                <span v-else-if="info.gender==='Female'" class="material-icons" >female</span>
                <span v-else class="material-icons" >remove</span>
                {{info.gender}}</td>
                <td>{{info.species}}</td>
                <td>{{info.episode}}</td>
                <td ><span class="material-icons md-48 favorite" v-on:click="addFav(info)" v-if="!data.fav.find(element=>JSON.stringify(info) === JSON.stringify(element))">
                    star_rate 
                    </span>
                    <span v-else class="material-icons md-48 favoriteClicked" >
                        star_rate 
                        </span>
                    </td>               
            </tr>
        </table>
        <div class="buttonContainer">
            <button v-for="(page, index) in data.pagesOnSearch" :key="page.index" v-on:click="fetchPageRick(page, index)" class="btn btn-info page">{{page}}</button>
        </div>
    </div>
    </div>
</template>

<script>
import {reactive, onBeforeMount} from "vue";
export default {
  name: "Ricky",
  setup() {
    const data= reactive({
    favShown: false,
    LastEpidoses: [],
    fav: [],
    infos: [],
    pagesOnSearch: 0,
    character: "",
    });
    onBeforeMount(fetchRick)
     function showFav() {
      data.favShown = true;
    }
    function showAll() {
      data.favShown = false;
    }
    function addFav(info) {
  

        data.fav.push(info);
      
    }
     async function fetchRick() {
      const response = await fetch(
        "https://rickandmortyapi.com/api/character/"
      );
      const res = await response.json();
      data.infos = res.results;
      data.pagesOnSearch = res.info.pages;
      countLastEpisodeArray();
    }
     async function fetchPageRick(page) {
      const response = await fetch(
        "https://rickandmortyapi.com/api/character/?name=" +
          data.character +
          "&page=" +
          page
      );
      const res = await response.json();
      data.infos = res.results;
      data.pagesOnSearch = res.info.pages;
      countLastEpisodeArray();
    }

    async function fetchSingleRick(character) {
      console.log(character)
      const response = await fetch(
        "https://rickandmortyapi.com/api/character/?name=" + character
      );
      const res = await response.json();
      data.infos = res.results;
      data.pagesOnSearch = res.info.pages;
      countLastEpisodeArray();

    }
    function countLastEpisodeArray() {
      data.LastEpidoses = [];
      data.infos.forEach((charactersInfo) => {
        var popped = charactersInfo.episode.pop();
        var last2 = popped.slice(-2);
        var number = last2.replace("/", "");
        //this.LastEpidoses.push(number);
        //console.log(this.LastEpidoses);
        charactersInfo.episode = number;
      });

    }
    return { data:data, fetchSingleRick:fetchSingleRick, fetchPageRick:fetchPageRick, fetchRick:fetchRick, addFav:addFav,showFav:showFav, showAll:showAll}
    
  }
}
</script>
<style scoped>
.top {
  border-bottom: 1px solid lightgrey;
  height: 120px;
  margin-left: 0px;
  margin-top: 50px;
}
p {
  float: left;
  padding: 30px;
}
.tabsContainer {
  padding-left: 10vw;

  overflow: hidden;
}
.charList {
  position: relative;
}
body {
  margin: 0px;
}
.active {
  color: #11b0c8;
  text-decoration: underline;
  text-underline-offset: 50%;
  text-decoration-style: solid;
}
.singleCharacter {
  border-bottom: 1px solid #e5eaf4;
  height: 200px;

  margin-left: 0px;
  padding-left: 6vw;
}

.charsAtributes {
  height: 70px;
  overflow: hidden;
  padding-left: 20vw;
  background-color: #e5eaf4;
  color: #a9b1bd;
}

.charImage {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 150px;
  height: 150px;
}
h5 {
  display: inline-block;
  color: #a9b1bd;
  padding: 70px 0;

  margin: 70px;
  float: left;
  text-align: center;
}
li button {
  margin: 60px;
}
.favorite {
  height: 60px;
  width: 60px;
  border: 1px solid#11b0c8;
  border-radius: 10%;

  text-align: center;
  padding: 4px 0;
  color: #11b0c8;
}
.favoriteClicked {
  height: 60px;
  width: 60px;
  border: 1px solid#11b0c8;
  border-radius: 10%;
  background-color: #11b0c8;
  text-align: center;
  padding: 4px 0;
  color: white;
}
tr {
  margin-left: 50px;
  border-color: #e5eaf4;
}
.photoAtr {
  margin-left: 70px;
}
table {
  color: #a9b1bd;
}
.logo {
  min-width: 200px;
  margin-top: -10px;
  float: left;
  margin-left: 50px;
  width: 20%;
  height: auto;
}
#input {
  margin-left: 200px;
  width: 600px;
}
.buttonContainer {
  margin: 10px;
}
.page {
  margin: 5px;
}
.material-icons.md-48 {
  font-size: 48px;
}

.inner-addon {
  top: 10px;
  margin-left: 30%;
  position: relative;
  width: 50%;
  margin-right: 50px;
  color: #11b0c8;
}

/* style glyph */
.inner-addon .glyphicon {
  position: absolute;
  padding: 10px;
  pointer-events: none;
}
.right-addon .glyphicon {
  right: 0px;
}
.right-addon input {
  padding-right: 30px;
}

</style>