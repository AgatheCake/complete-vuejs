<template>
  <pokemon-card
    :pokemons="pokemons"
    @chosen="fetchEvolutions"
    :selectedId="selectedId">
  </pokemon-card>

  <pokemon-card
    :pokemons="evolutions"
  </pokemon-card>
  </template>
  
  <script>
  import Card from './Card.vue'
  import PokemonCard from './PokemonCard.vue'


  const api = 'https://pokeapi.co/api/v2/pokemon'
  const IDS = [1, 4, 7]

  export default {
    components: {
      Card,
      PokemonCard
    },
    data() {
      return {
        pokemons: [],
        evolutions: [],
        selectedId: null
      }
    },
    async created() {
      this.pokemons = await this.fetchData(IDS)

    },
    methods: {
      async fetchEvolutions(pokemon) {
        console.log("pokemon dans fetchevolution", pokemon.id)
        this.evolutions = await this.fetchData([pokemon.id +1, pokemon.id +2])
        this.selectedId = pokemon.id
      },
      async fetchData(ids) {
      //maper 
        const responses = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)))
        const json = await Promise.all(responses.map(data => data.json()))

        return json.map(datum => ({
          id: datum.id, 
          name: datum.name,
          sprite: datum.sprites.other['official-artwork'].front_default,
          types: datum.types.map(element => element.type.name)
        }))
      }
    }


  };
  </script>
  
 <style scoped>

</style>