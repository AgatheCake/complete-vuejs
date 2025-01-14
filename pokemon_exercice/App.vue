<template>
<!--     <div>faire un bouton avec une fonction fetchData
      cette fonction appelera une api 
      on veut récupérer 3 pokemon seulements 
      et leur id, name, sprite, et type
      la fonction est asynchrone 
    </div> -->
    <pokemon-card
      :pokemons="pokemons"
      @chosen="fetchEvolution"
      :selectedId = "selectedId">
    </pokemon-card>

    <pokemon-card
      :pokemons="evolutions">
    </pokemon-card>

    <button @click="fetchOnePokemon">fetch One pokemon</button>
    <button @click="fetchData">fetch 3 pokemons</button>
</template>
  
<script>

  import Card from "./Card.vue"
  import PokemonCard from "./PokemonCard.vue"


  const api = 'https://pokeapi.co/api/v2/pokemon'
  const IDS = [1, 4, 7]

  export default {
    components: {
      Card,
      PokemonCard
    },
    data() {
      return {
        pokemon: null,
        pokemons: [],
        evolutions: [],
        selectedId: null
      }
    },
    methods: {
      async fetchEvolution(pokemon) {
        this.evolutions = await this.fetchData([pokemon.id +1,pokemon.id +2])
        this.selectedId = pokemon.id
      },
/*       async fetchOnePokemon() {
        const response = await window.fetch(`${api}/1`)
        const json = await response.json()

        this.pokemon = {
          id: json.id,
          name: json.name,
          types: json.types.map(e => e.type.name)
          
        }
        console.log(this.pokemon)
      }, */
      async fetchData(ids) {
        console.log("ids", ids)
        const response = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)))

        const json = await Promise.all(response.map(a => a.json()))

        return json.map(datum => ({
          id: datum.id,
          name: datum.name,
          sprite: datum.sprites.other['official-artwork'].front_default,
          types: datum.types.map(e => e.type.name)
        }))
      }
    },
    async created() {
      this.pokemons = await this.fetchData(IDS)
    }
  };
</script>
  
<style scoped>

  </style>