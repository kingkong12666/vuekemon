<template>
  <v-container>
    <v-layout row v-if="!loading">
      <v-flex xs12 sm6 offset-sm3>
        <v-card
          v-for="pokemon in pokemonData"
          class="elevation-10 mb-3"
        >
          <v-layout row>
            <v-flex xs4>
              <v-card-media
                :src="pokemon.sprites.front_default"
                height="160px"
              ></v-card-media>
            </v-flex>
            <v-flex xs8>
              <v-card-text>
                <h2 class="text--primary">{{pokemon.name}}</h2>
                <p
                v-for="ability in pokemon.abilities"
                >
                Ability: {{ability.ability.name}}

                </p>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  :to="'pokemon/' + pokemon.id"
                >Open</v-btn>
              </v-card-actions>
            </v-flex>
          </v-layout>
        </v-card>
      </v-flex>
    </v-layout>

    <v-layout row v-if="loading">
      <v-flex xs12 class="text-xs-center">
        <v-progress-circular
          :size="100"
          color="primary"
          indeterminate
        ></v-progress-circular>
      </v-flex>
    </v-layout>
  </v-container>
</template>





<script>
  import axios from 'axios'
  export default {
    data () {
      return {
        pokemonCount: null,
        pokemon: {
          name: null,
          image: null
        },
        pokemonData: {},
        promises: [],
        loading: true
      }
    },
    methods: {
    },
    mounted () {
      axios.get('https://pokeapi.co/api/v2/pokemon/')
        .then(response => (this.pokemonCount = response.data.count))
        .then(() => {
          for (var id = 1; id <= (this.pokemonCount - 940); id++) {
            this.promises.push(axios.get('https://cors.io/?https://pokeapi.co/api/v2/pokemon-form/' + id))
            this.promises.push(axios.get('https://cors.io/?https://pokeapi.co/api/v2/pokemon/' + id))
          }
          axios.all(this.promises).then((results) => {
            results.forEach((response) => {
              this.pokemonData[response.data.name] = response.data
            })
          })
            .finally(() => (this.loading = false))
        })

    }
  }
</script>

<style>
  body{
    background-color: antiquewhite;
  }
  v-btn{
    color: primary
  }
</style>
