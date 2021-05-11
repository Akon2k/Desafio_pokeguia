<template>
  <div class="container-fluid">
    <h1 class="text-center pt-3">PokeGuía</h1>
    <div class="mb-3 row justify-content-center">
      <div class="row">
        <div class="col-10">
          <input
            type="text"
            class="form-control"
            id="nombrePoke"
            v-model="nombrePoke"
            placeholder="Ingresa tu pokémon"
          />
        </div>

        <div class="col-2">
          <button @click="getData(nombrePoke)" class="btn btn-primary">
            Buscar
          </button>
        </div>
      </div>
    </div>
    <Card
      :movimientos="movimientos"
      :habilidades="habilidades"
      :url="url"
      :nombrePoke="nombrePokemon"
    />
  </div>
</template>

<script>
import Card from "./Card";

export default {
  name: "Body",
  data() {
    return {
      nombrePoke: "",
      response: null,
    };
  },
  methods: {
    async getData(name) {
      try {
        const nombre = name.toLowerCase();
        const url = `https://pokeapi.co/api/v2/pokemon/${nombre}`;
        const req = await fetch(url);
        const reqJSON = await req.json();
        this.response = reqJSON;
        console.log(reqJSON);
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    this.getData("pikachu");
  },
  computed: {
    movimientos() {
      if (this.response) return this.response.moves;
      return [];
    },
    habilidades() {
      if (this.response) return this.response.abilities;
      return [];
    },
    url() {
      if (this.response) return this.response.sprites.front_default;
      return "";
    },
    nombrePokemon() {
      if (this.response) return this.response.name;
      return "";
    },
  },
  components: {
    Card,
  },
};
</script>

<style scoped>
h1 {
  margin: 30px 0;
}
</style>
