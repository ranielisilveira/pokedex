<template>
  <div>
    <v-row class="mx-0 d-flex align-center">
      <template v-for="(evolutionDetail, index) in evolutions()">
        <v-col
          cols="12"
          md="3"
          :key="`evolution-${index}`"
          v-if="typeof evolutionDetail == 'object'"
        >
          <PokemonCard :pokemon="evolutionDetail" flat />
        </v-col>
        <v-col md="1" :key="`evolution-${index}`" v-else>
          <h3 class="text-center">L {{ evolutionDetail }}</h3>
        </v-col>
      </template>
    </v-row>
  </div>
</template>

<script>
import axios from "axios";
import PokemonCard from "./PokemonCard.vue";

export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      evolution: null,
    };
  },
  props: {
    pokemon: Object,
  },
  mounted() {
    this.fetchEvolution();
  },
  methods: {
    fetchEvolution() {
      axios
        .get(`https://pokeapi.co/api/v2/pokemon-species/${this.pokemon.id}`)
        .then((response) => {
          axios.get(response.data.evolutionChain.url).then((response) => {
            this.evolution = response.data.chain;
          });
        });
    },
    evolutions() {
      let chain = [];
      let evolution = this.evolution;
      if (evolution.species) {
        chain.push(evolution.species);
      }

      while (evolution.species) {
        if (evolution.evolvesTo.length > 0) {
          evolution = evolution.evolvesTo[0];

          if (
            evolution.evolutionDetails.length > 0 &&
            evolution.evolutionDetails[0].minLevel
          ) {
            chain.push(evolution.evolutionDetails[0].minLevel);
          }

          if (evolution.species) {
            chain.push(evolution.species);
          }
        } else {
          break;
        }
      }

      return chain;
    },
  },
  watch: {
    pokemon() {
      this.fetchEvolution();
    },
  },
};
</script>

<style lang="scss" scoped></style>
