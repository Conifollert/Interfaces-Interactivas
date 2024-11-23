<script>

export default {
    props: ["pokemons", "index"],
    data(){
      return {
        nombre: ""
      }
    }
};

</script>

<template>
    <div class="pokemoncard">
        <img :class="{oculta:!pokemons.descubierto}" :src="pokemons.imagen" alt="">
        <input v-if="!pokemons.descubierto" class="input" type="text" v-model="nombre">
      <button class="btn-descrubir" v-if="!pokemons.descubierto" @click="$emit('checkGuess', {index, nombre})">
        Descubrir
      </button>
      <p v-if="pokemons.descubierto">{{ pokemons.nombre }}</p>
    </div>
  </template>
  

  <style scoped>
  h1 {
    color: #333;
  }

  .pokemoncard{
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .input{
    width: 100px;
  }

  .btn-descrubir{
    margin-top: 10px;
  }

  .oculta{
    filter: blur(5px) grayscale(100%);
  }
  </style>
  