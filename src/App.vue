<template>
  <div class="container" id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="https://logodownload.org/wp-content/uploads/2017/08/pokemon-logo.png" alt="Pokemon Logo" width="640">
      <hr>
      <h4 class="is-size-4" style="color:red;">Pokedex</h4>
      
      <input class="input is-rounded" type="text" name="search-pokemon" id="buscaForm" placeholder="Search Pokemon" v-model="busca">
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Search</button>
      
      <hr>
      <div v-for="(pokemon) in filteredPokemons" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :url="pokemon.url"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';


export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    }).catch((err) => {
      console.log(err);
    });
  },
  components: {
    Pokemon
  }, 
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' ')
      {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  },
  computed: {
    resultSearch: function(){
      if(this.busca == '' || this.busca == ' ')
      {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }  
    }
  }
}
</script>

<style>
#app {
  margin: 0 auto;
  padding: 0 auto;
  text-align: center;
  border-top: 60px;
}

#buscaForm{
  margin-top: 2%;
}

#buscaBtn{
  margin-top: 2%;
}
</style>
