<template>
  <v-app>
    <v-container>
      <v-card>
        <v-container>
          <v-row>
            <v-col cols="2" v-for="pokemon in pokemons.slice(0, 50)" :key="pokemon.name">
              <v-card>
                <v-container>
                  <v-row class="mx-0 d-flex justify-center">
                    <img
                      :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getPokemonId(pokemon)}.png`"
                      :alt="pokemon.name" width="80%" />
                  </v-row>
                  <h2 class="text-center">{{ pokemon.name }}</h2>
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

import axios from 'axios';

export default {
  name: 'App',

  components: {},

  data() {
    return {
      pokemons: []
    }
  },

  mounted() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151')
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },

  methods: {
    getPokemonId(pokemon) {
      return Number(pokemon.url.split('/')[6]);
    }
  }

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
