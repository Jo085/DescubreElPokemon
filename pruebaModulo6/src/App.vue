<script >
import axios from "axios";
import PokeBox from "./components/PokeBox.vue";

export default{
  name: 'App',
  data(){
    return{
     pokemones: [],
     urlImage:"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/", 
     contador: [],      
    };
  },

  components:{
     PokeBox,
  }, 

   methods:{
     
    async getPokemones() {
      try {
        const url = "https://pokeapi.co/api/v2/pokemon" //url API
        const {data} = await axios.get(url); //consulta API
              
        data.results.forEach((pokemon) => {         
            pokemon.id = pokemon.url.slice(34, -1); // se crea un id para cada pokemon a partir de la  propiedad url    
            pokemon.input = ""; // se crea propiedad que recibe el valor del input 
            pokemon.show = false; // propiedad que será usada para evaluar si mostrar o no al pokemon
            this.pokemones.push(pokemon); // agrega cada objeto Pokemon al array Pokemones          
        });  
      } catch(error) {
            console.log(error);
        }       
    },

    descubrir(pokemon) {
      // Si el nombre del pokemon coincide con el valor ingresado por el usuario      
      if(pokemon.name.toLowerCase() == pokemon.input.toLowerCase() ){ 
        // El atributo 'show' de pokemón cambia a Verdadero        
        pokemon.show = true;     
        // y se agrega al arreglo Contador
        this.contador.push(pokemon.show);
        // Si el nombre no coincide se muestra una alerta y limpia el campo 
      } else{       
        alert("El nombre ingresado es incorrecto");
        pokemon.input = "";
      }
    },
  },

  mounted() { 
    // LLamado a la función que traerá los datos de la consulta 
    // Se ejecutará al cargar la página
    this.getPokemones();
  },

  computed:{
    imgPokemones(){  
      // devuelve un array con las fotos de los pokemones 
      // concatena la url de la imagen, el id del pokemon y su formato  
      return this.pokemones.map((pokemon) => this.urlImage + pokemon.id + ".svg");
    },    
    
    contadorPokemones(){
      //retorna la cantidad de elementos del arreglo Contador       
      return this.contador.length;
    }
  },
};
</script>

<template>

  <div >
    <div class="text-center mb-4"> 
    <!-- Imagen logo pokemon -->
      <img src="https://pngimg.com/uploads/pokemon_logo/pokemon_logo_PNG3.png" class="logoPokemon mb-3" alt="">    
      <!-- Título -->
      <h1>¿Quién es ese Pokémon?</h1>
      <!-- Contador Pokemones-->
      <p>Pokemones descubiertos: <span class="text-warning fs-5 fw-bold mx-2" >{{ contadorPokemones }}</span></p>
    </div>   

    <!-- Contenedor Pokemones -->
    <div class="d-flex flex-wrap justify-content-around container gap-3 mb-5">      
      <!-- Componente Card Pokemon  -->
      <PokeBox v-for="(pokemon, index) in pokemones " :key="pokemon.index" 
      :imgPokemones="imgPokemones[index]"
      :pokemon.name
      :pokemon.input
      :pokemon.show
      :pokemon.id       
      :contadorPokemones
      @descubrirPokemon="descubrir(pokemon)"    
      />        
    </div>  
  </div>
</template>

<style scoped>

.logoPokemon{
   width: 20vw;
}

</style>
