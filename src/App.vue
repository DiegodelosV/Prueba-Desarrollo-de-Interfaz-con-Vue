<template>
  <div id="app" class="container">
    <header class="text-center">
      <img
        src="./assets/img/pokemon-logo.png"
        class="img-fluid my-3"
        alt="Pokemon Logo"
        id="logo"
      />
      <h1 class="fw-bold">¿Quién es ese Pokémon?</h1>
      <p class="text-muted">
        Pokémones descubiertos: <span class="TextoDescubiertos">{{ cantidadDescubiertos }}</span>
      </p>
    </header>

    <div class="row">
      <div class="col-12 col-md-3" v-for="(pokemon, i) in pokemones" :key="i">
        <PokemonCard
          :nombre="pokemon.nombre"
          :imagen="pokemon.imagen"
          @pokemon-descubierto="marcarComoDescubierto(i)"
          :es-descubierto="pokemon.descubierto"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";

export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemones: [], // Array para almacenar los datos de Pokémon
    };
  },
  async mounted() {
    await this.obtenerPokemones(); // Llamar a la función para obtener los datos y actualizar el estado
  },
  methods: {
    obtenerPokemones() {
      axios
        .get("https://pokeapi.co/api/v2/pokemon?limit=20")
        .then((response) => {
          const resultados = response.data.results;
          resultados.forEach((pokemon) => {
            axios.get(pokemon.url).then((resultado) => {
              this.pokemones.push({
                nombre: pokemon.name,
                imagen: resultado.data.sprites.other.dream_world.front_default,
                descubierto: false,
              });
            });
          });
        })
        .catch((error) => console.error(error, "Error al obtener los datos"));
    },
    marcarComoDescubierto(i) {
      this.pokemones[i].descubierto = true;
    },
  },
  computed: {
    cantidadDescubiertos() {
      // para contar los pokemones descubiertos
      return this.pokemones.filter((pokemon) => pokemon.descubierto).length;
    },
  },
};
</script>
