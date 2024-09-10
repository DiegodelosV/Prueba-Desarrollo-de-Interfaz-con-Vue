<template>
  <div class="BoxPokemon card text-center">
    <img
      :src="imagen"
      class="card-img-top"
      :class="{ desenfocar: !revelado }"
      alt="pokemon"
    />
    <div class="card-body">
      <div v-if="!revelado">
        <input
          v-model="entradaPokemon"
          @keyup.enter="descubrirPokemon"
          class="form-control"
          placeholder="Nombre del Pokémon"
        />
        <button @click="descubrirPokemon" class="btn btn-success mt-2">
          Descubrir
        </button>
      </div>
      <p class="fw-bold fs-5" v-else>{{ nombre }}</p>
    </div>

    <!-- Modal de alerta de error -->
    <div
      class="modal fade"
      id="modalError"
      tabindex="-1"
      aria-labelledby="modalErrorLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="modalErrorLabel">Fallaste!</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            Nombre de Pokemón incorrecto, intenta nuevamente.
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Cerrar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  props: {
    nombre: String,
    imagen: String,
  },
  data() {
    return {
      entradaPokemon: "",
      revelado: false,
    };
  },
  methods: {
    descubrirPokemon() {
      if (this.entradaPokemon.toLowerCase() === this.nombre.toLowerCase()) {
        this.revelado = true;
        this.$emit("pokemon-descubierto");
      } else {
        // Mostrar el modal de error
        const modalError = new bootstrap.Modal(
          document.getElementById("modalError")
        );
        modalError.show();
      }
    },
  },
};
</script>

<style scoped>
.BoxPokemon {
  margin: 2em;
  text-align: center;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.desenfocar {
  filter: blur(5px) grayscale(100%);
}

img {
  margin: 1em auto 0;
  width: 10rem;
  height: 10rem;
  padding: 1em;
}

</style>
