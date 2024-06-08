<template>
  <div class="center-container">
    <h1  class="img" style="text-align: center;">Pokémon Search</h1>
    <div class="search-container">
      <input type="text" v-model="searchTerm" @input="searchPokemon" placeholder="Enter Pokemon Name" class="search-input"/>
      <button @click="searchPokemon">Submit</button>
      <button @click="resetPokemon">Reset</button>
      <div v-if="pokemon" class="pokemon-info">
        <h2>Name: {{ pokemon.name }}</h2>
        <img :src="pokemon.imageUrl" :alt="pokemon.name" class="pokemon-image" />
        <h3>Abilities:</h3>
        <ul>
          <li v-for="ability in pokemon.abilities" :key="ability.ability.name">{{ ability.ability.name }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      searchTerm: '',
      pokemon: null
    }
  },
  methods: {
    async searchPokemon () {
      if (this.searchTerm === '') {
        this.pokemon = null
        return
      }

      try {
        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.searchTerm.toLowerCase()}`)
        const data = await response.json()
        const imageUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${data.id}.png`
        data.imageUrl = imageUrl
        this.pokemon = data
      } catch (error) {
        console.error('Error fetching data:', error)
        this.pokemon = null
      }
    },
    resetPokemon () {
      this.searchTerm = '' // Clear the search term
      this.pokemon = null
    }
  }
}
</script>

<style>
.center-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-image: url('./assets/p11.jpg');
}

.search-container {
  text-align: center;
  margin: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  background-color: #fff;
}
</style>
