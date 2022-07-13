<template>
  <v-app>
    <v-container>
      <v-card>
        <v-container>
          <v-text-field v-model="search" label="Pesquisar" placeholder="Pesquisar Pokemon" solo></v-text-field>
          <v-row>
            <v-col cols="2" v-for="pokemon in filteredPokemons" :key="pokemon.name">
              <v-card>
                <v-container>
                  <v-row class="mx-0 d-flex justify-center">
                    <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getPokemonId(
                      pokemon
                    )}.png`" :alt="pokemon.name" width="80%" />
                  </v-row>
                  <h2 class="text-center">{{ getPokemonName(pokemon) }}</h2>
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  components: {},

  data() {
    return {
      pokemons: [],
      search: ""
    };
  },

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151")
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },

  methods: {
    getPokemonId(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },

    getPokemonName(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
  },

  computed: {
    filteredPokemons() {
      return this.pokemons.filter((pokemon) => {
        return pokemon.name.includes(this.search);
      });
    }
  },
};
</script>

<style>
#app {
  background: linear-gradient(to bottom right,
      rgba(10, 10, 10, 1),
      rgba(12, 39, 63, 1)) no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>
