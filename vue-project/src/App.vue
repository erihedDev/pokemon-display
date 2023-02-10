<script>
const url = "./pokemon/"

export default{
  data(){
    return{
      pokemons: [],
      imgUrl: ""
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch('../data/data.json')
        .then(response => response.json())
        .then(json => {
          this.pokemons = json;
        });
    },
    getImageUrl(id, form) {
      if (form !== " ") {
        const formText = form.toLowerCase().split(" ");
        if (formText.length >= 3) {
          return url + id + "-" + formText[0] + "-" + formText[2] + ".png";
        } 
        
        else {
          let returnString = formText[0]
          if (returnString === "male" || returnString === "female") {
            return url + id + ".png";
          }
          else if (returnString === "alolan") {
            returnString = "alola";
          } 
          else if (returnString === "galarian") {
            returnString = "galar";
          }
          else if (returnString === "hisuian") {
            return url + id + ".png";
          }

          return url + id + "-" + returnString + ".png";
        }
      }

      return url + id + ".png";
    },
  }
}
</script>

<template>
  <div class="body">
    <div class="container">
      <div v-for="pokemon in pokemons" class="card">
        <div class="card-header">
            <p class="pokemon-name">{{ pokemon.Name }}</p>
            <p>#{{ pokemon.ID }}</p>
        </div>
        <div class="card-types">
          <p :class="pokemon.Type1">{{ pokemon.Type1 }}</p>
          <p v-if="pokemon.Type2 !== ' '" :class="pokemon.Type2">{{ pokemon.Type2 }}</p>
        </div>
        <p v-if="pokemon.Form !== ' '" class="pokemon-form">{{ pokemon.Form }}</p>
        <div class="card-image">
          <img :src="getImageUrl( pokemon.ID, pokemon.Form )" alt="Missing sprite of pokemon"/>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.body {
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 1560px;
}

.card {
  color: white;
  font-family: 'Lato', sans-serif;
  font-size: 18px;
  width: 200px;
  height: 240px;
  background-color:  	#383838;
  margin: 12px;
  padding: 12px;
  border-radius: 12px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  height: 40px;
}

.pokemon-name {
  font-weight: bold;
}

.card-types {
  display: flex;
}
.card-types p {
  height: 24px;
  margin-right: 10px;
  padding: 2px 10px;
  border-radius: 4px;
}

.pokemon-form {
  background-color: #686868;
  margin-top: 10px;
  height: 24px;
  width: fit-content;
  padding: 2px 10px;
  border-radius: 4px;
}

.card-image {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 140px;
}

img {
  margin-top: 10px;
  width: 100px;
  height: 100px;
}
</style>
