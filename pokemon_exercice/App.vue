<template>
<!--     <div>faire un bouton avec une fonction fetchData
      cette fonction appelera une api 
      on veut récupérer 3 pokemon seulements 
      et leur id, name, sprite, et type
      la fonction est asynchrone 
    </div> -->
   
<div class="cards">
<card
    v-for="pokemon in pokemons"
    :key="id"
    :pokemon="pokemon">

  <template v-slot:title>
    {{ pokemon.name }}
  </template>

  <template v-slot:content>
    <img :src="pokemon.sprite" />
  </template>

  <template v-slot:description>
    <div v-for="type in pokemon.types">
        {{type}}
    </div>
  </template>
  
  </card>

   
  </div>
    <button @click="fetchOnePokemon">fetch One pokemon</button>
    <button @click="fetchThreePokemon">fetch 3 pokemons</button>
  </template>
  
  <script>

  import Card from "./Card.vue"

  const api = 'https://pokeapi.co/api/v2/pokemon'
  const ids = [1, 4, 7]

  export default {
    components: {
      Card
    },
    data() {
      return {
        pokemon: null,
        pokemons: []
      }
    },
    methods: {
      async fetchOnePokemon() {
        const response = await window.fetch(`${api}/1`)
        const json = await response.json()

        this.pokemon = {
          id: json.id,
          name: json.name,
          types: json.types.map(e => e.type.name)
          
        }
        console.log(this.pokemon)
      },
      async fetchThreePokemon() {
        console.log("fetch 3 pokemon ")
        const response = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)))

        const json = await Promise.all(response.map(a => a.json()))

        this.pokemons = json.map(datum => ({
          id: datum.id,
          name: datum.name,
          sprite: datum.sprites.other['official-artwork'].front_default,
          types: datum.types.map(e => e.type.name)
        }))


        console.log("this.pokemons", this.pokemons)
      }
    },
    created() {
      this.fetchThreePokemon()
    }
  };
  </script>
  
  <style scoped>
  .cards {
  display: flex
}
img { 
  width: 100%
}
  .card {
    border: 1px solid silver;
    border-radius: 8px;
    max-width: 200px;
    margin: 0 5px;
    cursor: pointer;
    box-shadow: 0px 1px 3px darkgrey;
    transition: 0.2s;
  }
  .title, .content, .description {
    padding: 16px;
    text-transform: capitalize;
    text-align: center;
  }
  .title, .content {
    border-bottom: 1px solid silver;
  }
  .title {
    font-size: 1.25em;
  }
  .card:hover {
    transition: 0.2s;
    box-shadow: 0px 1px 9px darkgrey;
  }
  </style>