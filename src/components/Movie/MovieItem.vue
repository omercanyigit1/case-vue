<template>
    <v-card
      class="mx-auto"
      max-width="400"
    >
      <v-img
        class="align-end text-white"
        height="200"
        :src="`${movie.show.image.medium}`"
        cover
      >
        <v-card-title class="movie-title">{{ movie.show.name }}</v-card-title>
      </v-img>
  
      <v-card-subtitle class="pt-4">
        <v-chip
            class="ma-2"
            :color="movie.score * 100 < 70 ? 'error' : 'success'"
            variant="outlined"
            >
            <v-icon :icon="movie.score * 100 < 70 ? 'mdi-arrow-down-bold-box-outline' : 'mdi-arrow-up-bold-box-outline'" start></v-icon>
            {{ formattedScore(movie.score * 100) }}
        </v-chip>
        <v-chip
            class="ma-2"
            color="primary"
            variant="outlined"
            >
            <v-icon icon="mdi-translate" start></v-icon>
            {{ movie.show.language }}
        </v-chip>
      </v-card-subtitle>
  
      <v-card-text class="v-card-detail-item">
        <div class="flex-item">
            <div v-for="genre in movie.show.genres" :key="`genre-${movie.show.name}-${genre}`">
                <v-chip color="primary">
                {{ genre }}
            </v-chip>
            </div>
        </div>
        <div class="movie-summary" v-html="movie.show.summary" />
      </v-card-text>
  
      <v-card-actions>
        <v-btn color="orange" text="Detail" :to="{ name: 'detail', params: { id: movie.show.id } }"></v-btn>
      </v-card-actions>
    </v-card>
  </template>

<style>
.movie-title {
    background-color: rgba(0, 0, 0, .7);
}

.v-card-detail-item {
    overflow-y: hidden;
    text-overflow: ellipsis;
    height: 220px;
}
.flex-item {
    display: flex;
    margin-bottom: 15px;
}

.flex-item .v-chip {
    margin-right: 5px;
}

</style>

<script>
export default {
    props: {
        movie: {
            type: Object,
            required: true
        }
    },

    methods: {
        formattedScore(score) {
            return score.toFixed(0);
        },
    }
}
</script>