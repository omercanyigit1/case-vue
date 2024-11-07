<script setup>
import MovieItem from './MovieItem.vue';
import { debounce } from 'lodash'; // Lodash debounce fonksiyonu
</script>

<template>
    <div v-if="moviesError">
        <v-alert
            color="error"
            icon="$error"
            title="Hata Aldınız!"
            :text="`${ moviesError }`"
        ></v-alert>
    </div>
    <v-container>
        <v-row>
            <v-col sm="12">
                <v-card
                    class="mx-auto"
                    color="surface-light"
                    max-width="400"
                >
                    <v-card-text>
                    <v-text-field
                        :loading="moviesLoading"
                        append-inner-icon="mdi-magnify"
                        density="compact"
                        label="Search Show"
                        variant="solo"
                        v-model="searchQuery"
                        clearable
                        hide-details
                        single-line
                    ></v-text-field>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <div v-if="moviesLoading">
                <v-progress-circular
                :size="50"
                color="primary"
                indeterminate
                ></v-progress-circular>
            </div>
            <v-col
            v-for="(movie, index) in movies"
            :key="index"
            cols="12"
            sm="3"
            >
            <MovieItem :movie="movie" />
            </v-col>
      </v-row>
    </v-container>
</template>

<script>
    import axios from 'axios';
    
    export default {
        data() {
            return {
                movies: [],
                moviesLoading: false,
                moviesError: null,
                searchQuery: '',
                dialog: false,
            };
        },

        mounted() {
            this.fetchMoviesDebounced = debounce(this.fetchMovies, 500);
        },

        methods: {
            async fetchMovies(query) {
                try {
                    const response = await axios.get(`https://api.tvmaze.com/search/shows`, {
                        params: {
                            q: query,
                        }
                    });

                    this.movies = response.data;
                    this.moviesLoading = false; 

                } catch (error) {
                    this.moviesError = error.message;
                    this.moviesLoading = false;
                }
            }
        },

        watch: {
            searchQuery(newQuery) {
                if(newQuery) {
                    this.fetchMoviesDebounced(newQuery);
                }
            }
        },
    }
</script>
