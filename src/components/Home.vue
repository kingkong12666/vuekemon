<template>
  <v-container>
    <v-layout row v-if="!loading">
      <v-flex xs12 sm6 offset-sm3>
        <v-data-iterator
          :items="pokemonData"
          :pagination.sync="pagination"
          hide-actions>
          <v-flex slot="item" slot-scope="props">
            <v-card
              class="elevation-10 mb-3"
            >
              <v-layout row>
                <v-flex xs4>
                  <v-card-media
                    :src="props.item.sprites.front_default"
                    height="160px"
                  ></v-card-media>
                </v-flex>
                <v-flex xs8>
                  <v-card-text>
                    <h2 class="text--primary">{{props.item.name }}</h2>
                    <p
                    >
                    </p>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      :to="'pokemon/' + props.item.id"
                    >Open</v-btn>
                  </v-card-actions>
                </v-flex>
              </v-layout>
            </v-card>
          </v-flex>
        </v-data-iterator>
        <v-pagination
          v-model="pagination.page"
          :length="totalPages"
        ></v-pagination>
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
        pagination: {
          page: 1,
          rowsPerPage: 3
        },
        pokemonCount: null,
        pokemonData: [],
        promises: [],
        loading: true
      }
    },
    methods: {
    },
    computed: {
      totalPages() {
        return (233)
      }
    },
    mounted () {
      axios.get('https://pokeapi.co/api/v2/pokemon/')
        .then(response => (this.pokemonCount = response.data.count))
        .then(() => {
          for (var id = 1; id <= (this.pokemonCount - 250); id++) {
            this.promises.push(axios.get('https://cors.io/?https://pokeapi.co/api/v2/pokemon-form/' + id))
            /*this.promises.push(axios.get('https://cors.io/?https://pokeapi.co/api/v2/pokemon/' + id))*/
          }
          axios.all(this.promises).then((results) => {
            results.forEach((response) => {
              this.pokemonData.push(response.data)
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
