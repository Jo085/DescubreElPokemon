<script>
export default{    
    props: [
      "pokemon",
      "imgPokemones",   
      "contadorPokemones",   
    ],    
};
</script>

<template>    

  <!-- Card -->
  <div class="card d-flex align-items-center rounded-5 mb-3"
  :class="{descubierto: pokemon.show}"
  >    
      <!-- Imagen Pokemon -->
        <img id="PokeImg" class=" img-fluid p-5 pb-3" 
        :src="imgPokemones" 
        :class="{
          filtro: !pokemon.show, 
          pokeDance: contadorPokemones == 20,            
        }"
        :alt="pokemon.name" 
        style="height: 14rem;"> 
           
    <div class="card-body d-flex text-center align-items-center">
         <!-- Texto Nombre Pokemon -->
        <p class="m-0 pokeName" v-if="pokemon.show"> {{  pokemon.name }}</p>
        <div v-if="!pokemon.show">
          <!-- Input -->
          <input class="form-control" @keypress.enter="$emit('descubrirPokemon', {name, input, show})"
            type="text" :id="pokemon.id"  v-model.trim="pokemon.input">
          <!-- Botón -->
          <button @click="$emit('descubrirPokemon', {name, input, show})" 
            class="btn btn-danger border  col-12 mt-3 text-withe">
            Descubrir
          </button> 
        </div>     
    </div>
  </div>

</template>

<style>
.card{
  width: 13rem;  
} 

.filtro{
  filter: blur(5px) grayscale(100%);
}

.descubierto{ 
  border: 1px solid #3564AE;
  box-shadow: 4px 5px 5px 0px grey;
}

.pokeName{
  font-size: 1.7rem;
  font-family: sans-serif;
  font-weight: 700;
  color: #3564AE;  
}

#PokeImg{ 
  mix-blend-mode: multiply;
}

.pokeDance{
  animation-name: dance;
  animation-duration: 4s;
  filter: brightness(1.1);
}

@keyframes dance{
  0% {translate: 0px 0px;}
  25% {translate: 30px 0px; rotate: y -180deg ;}
  50% {translate: -30px 30px; rotate: 0deg;}
  75% {translate: 0px -30px; rotate: y 180deg}
  100% {translate: 0px 0px;}
}

</style>