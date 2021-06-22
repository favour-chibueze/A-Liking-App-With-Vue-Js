<template>
   <div id="app" class="app"> 
      <h1 class="title"> Anime Characters</h1>
      <img :src="currentAnimeCharacter" alt="Anime Character" class="card"/>
      <button @click="loadAnime" class="nope">Next</button>
      <button @click="addFave" class="fav">Fave</button>

      <section class="favourites">
        <h2>Favourite Anime Characters</h2>
        <ul class="favourites-list">
          <li v-for="(anime,i) in favourites" :key="i" class="favourites-item">
            <img :src="anime" class="favourites-img" />
            <button @click="removeFave(anime)" class="remove">Remove</button>
          </li>        
        </ul>
      </section>
    </div>
</template>

<script>

export default {
  el: '#app',
  data() {
    return {
    currentAnimeCharacter: null,
    favourites: []
    }
  },
  methods: {
    loadAnime: async function() {
      const response = await fetch('https://api.waifu.pics/sfw/smile');
      const asJson = await response.json();
      this.currentAnimeCharacter = asJson.url;
    },
    addFave: function() {
      if (!this.favourites.includes(this.currentAnimeCharacter)) {
        this.favourites.push(this.currentAnimeCharacter);
      }
      this.loadAnime();      
    },
    removeFave: function(anime) {
      this.favourites = this.favourites.filter(item => item !== anime);
    }
  },
  created() {
    this.loadAnime();
  }
};
</script>

<style>
body {
  margin: 0;
  font-family: arial;
  background-color: #f2f2f2;
}

.app {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto 500px 100px 1fr;
  grid-template-areas:
    "title title"
    "card card"
    "nope fav"
    "favourites favourites";
}

.title {
  grid-area: title;
  font-size: 2em;
  color: black;
  text-align: center;
}

.card {
  grid-area: card;
  display: block;
  width: 100%;
  max-width: 400px;
  height: 500px;
  margin: auto;
  border-radius: 10px;
  object-fit: cover;
  overflow: hidden;
  background-color: white;
}

button {
  width: 70px;
  height: 70px;
  margin: 20px 5px;
  border: 8px solid white;
  border-radius: 50%;
  text-align: center;
  font-weight: bold;
  color: transparent;
  cursor: pointer;  
}

.nope {
  grid-area: nope;
  background: url('~@/assets/delete.svg') tomato center no-repeat;
  background-size: 20px;
  justify-self: end;
}

.nope:hover,
.remove:hover {
  background-color: #da553d;
}

.fav {
  grid-area: fav;
  background: url('~@/assets/like.svg') no-repeat center #63dd9a;
  background-size: 26px;
 
}

.fav:hover {
  background-color: #51c384;
}

h2 {
  color: black;
  text-align: center;
}

.favourites {
  grid-area: favourites;
  overflow-y: auto;
}

.favourites-list {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  padding: 0;
  margin: 0;
}

.favourites-item {
  position: relative;
  list-style: none;
  margin: 15px;
}

.favourites-img {
  width: 150px;
  height: 150px;
  object-fit: cover;
}

.favourites-img:hover {
  width: 350px;
  height: 250px;
  cursor: pointer;
}

.remove {
  position: absolute;
  bottom: -8px;
  right: -15px;
  height: 30px;
  width: 30px;
  margin: 0;
  border: 2px solid white;
  background: url('~@/assets/delete.svg') tomato center no-repeat;
  background-size: 17px;
}

</style>
