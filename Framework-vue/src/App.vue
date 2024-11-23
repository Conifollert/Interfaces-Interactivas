<script>
import axios from 'axios';
import PokemonList from './components/PokemonList.vue';
import { computed } from 'vue';
 
export default {

components: {
 PokemonList
},

  data() {
    return {
      pokemons: [],
      pokemonName: "",
      discoveredCount: 0,
      discovered: false,
      discoveredPokemon: [],
      errorMessage: ""
    };
  },
  methods: {

    verificarNombre({index, nombre}){
    const pokemon = this.pokemons[index];
    console.log(nombre)
    if (nombre.toLowerCase() === pokemon.nombre.toLowerCase()){
      this.pokemons[index].descubierto = true;
    } else{
      alert('')
    }
   },

    async axiosPokemons() {
      try {
        const {data} = await axios('https://pokeapi.co/api/v2/pokemon?limit=20');

        let listaPokemones = data.results;
        listaPokemones.forEach(async ({url}) =>{
          const response = await axios.get(url)

          let poke = response.data
          let pokemon = {
            nombre: poke.name,
            imagen: poke.sprites.front_default,
            descubierto: false
          }
          this.pokemons.push(pokemon)
        })
        
      } catch (error) {
        console.error("Error fetching Pokémon:", error);
      }
    },
    discoverPokemon() {
      const pokemon = this.pokemons.find(p => p.name === this.pokemonName.toLowerCase());

      if (pokemon) {
        // Pokémon encontrado
        this.discovered = true;
        this.discoveredPokemon = pokemon.name;
        this.discoveredCount++;
        this.errorMessage = "";
      } else {
        // Error: nombre incorrecto
        this.errorMessage = "El nombre ingresado es incorrecto";
      }
    }
  },
  mounted() {
    // Llamar la API cuando el componente se monta
    this.axiosPokemons();
  },
   computed:{
    descubiertos(){
      return this.pokemons.filter((poke)=>!!poke.descubierto).length
    }
   },
    
};



</script>

<template>
<div>
    <img class="logo" src="./assets/img/logo-pokemon.svg" alt="">
    <h1>¿Quién es ese Pokemon?</h1>
    <p>Pokemones Descubiertos {{ descubiertos }}</p>

    <div class="grid">
      <div class="card-pokemon" v-for="(pokemon,index) in pokemons" :key="index"> 
        <PokemonList @checkGuess="verificarNombre" :pokemons="pokemon" :index="index"></PokemonList>
      </div>
    </div>

    
    <!-- <input v-if="!discovered" v-model="pokemonName" type="text" placeholder="Ingresa el nombre del pokémon" /> -->
    <p v-if="discovered">¡Has descubierto a {{ discoveredPokemon }}!</p>
    <!-- Mensaje de error -->
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>



</template>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.logo {
  width: 300px;
}
 .grid{
  margin-top: 40px;
  display: grid;
  grid-template-columns: repeat(4,1fr);
  gap: 20px;
 }

</style>
