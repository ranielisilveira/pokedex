<template>
  <v-card @click="showPokemon(getId(pokemon))" :flat="flat">
    <v-container>
      <v-row class="mx-0 d-flex justify-center">
        <img
          :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getId(
            pokemon
          )}.png`"
          :alt="pokemon.name"
          width="80%"
        />
      </v-row>
      <h2 class="text-center">{{ getName(pokemon) }}</h2>
    </v-container>
  </v-card>
</template>

<script>
export default {
  props: {
    pokemon: Object,
    flat: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    getId(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },
    getName(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
    showPokemon(id) {
      this.$emit("clicked", id);
    },
  },
};
</script>

<style lang="scss" scoped></style>
