<template>
  <div id="app">

    <div class="column is-half is-offset-one-quarter">

      <h1>Pokedex</h1>
      <input class="input is-rounded" type="text" placeholder="Buscar pokémon pelo nome" v-model="busca">
      <button id="buscaBtn" class="button is-fullwidth is-success" @click="buscar">Buscar</button>
      <hr>

      <div v-for="(poke) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="poke.url | pokenumber"/>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    });
  },
  components: {
    Pokemon
  },
  methods: {
    buscar: function() {
      this.filteredPokemons = this.pokemons;
      if (this.busca == '' || this.busca == ' ') {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  },
  computed: {
    /*
    resultadoBusca: function() {
      if (this.busca == '' || this.busca == ' ') {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }*/
  },
  filters: {
    pokenumber: function(value) {
      var pokenumber = value.split("/");
      var number = pokenumber.length - 2;
      return parseInt(pokenumber[number]);  
    }
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#buscaBtn {
  margin-top: 2%;
}
</style>
