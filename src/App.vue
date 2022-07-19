<template>
  <v-app>
    <v-container>
      <div>
        <br /><br /><br /><br /><br /><br />
        <v-col cols="12" sm="6">
          <h1>Discover all abilities of<br /> a Pokemon</h1><br />
          <v-row no-gutters align="center" justify="center">
            <v-col cols="12" sm="12">
              <div class="d-flex">
                <v-text-field class="pt-1 mr-2" dense outlined clearable placeholder="Enter pokemon name (In English)"
                  v-model="pokemonName">
                </v-text-field>
                <v-btn elevation="2" class="ma-2" outlined color="blue" :loading="loading" @click="searchAbilities()">
                  Pesquisar</v-btn>
              </div>
            </v-col>
          </v-row>
          <v-row no-gutters justify="left">
            <v-list>
              <v-list-item v-for="i in abilities" :key="i">
                <v-list-item-content>
                  <v-list-item-title> {{ i }} </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-row>
        </v-col>
        <v-col></v-col>
        <v-snackbar v-model="snackbar">
          {{ text }}

          <template v-slot:action="{ attrs }">
            <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
              Close
            </v-btn>
          </template>
        </v-snackbar>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  components: {},

  data: () => ({
    pokemonName: "",
    snackbar: false,
    loading: false,
    abilities: [],
    text: '',
  }),

  methods: {
    searchAbilities: async function () {
      if (this.pokemonName) {

        const url = `http://localhost:3000/api/pokemons/${this.pokemonName.toLowerCase()}`
        this.loading = !this.loading;
        try {
          const response = await axios.get(url)
          if (response.status == 200) {
            this.abilities = response.data
          } else {
            this.abilities = [];
          }
        } catch (error) {
          this.abilities = [];
          this.text = error.response.data.error;
          this.snackbar = true;
        }
        this.loading = !this.loading;
      } else {
        this.abilities = [];
        this.text = "To discover abilities, please type a pokemon name";
        this.snackbar = true;
      }
    },
  },
  watch: {
    loader() {
      const l = this.loader
      this[l] = !this[l]

      setTimeout(() => (this[l] = false), 3000)

      this.loader = null
    },
  },
};
</script>

<style>
#app {
  background: url('./assets/wallpaper-pokemon-1024x768-wallpaper.jpg') no-repeat right;
  background-size: 100% 100%;
}
</style>