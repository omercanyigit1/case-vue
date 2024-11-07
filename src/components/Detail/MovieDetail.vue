<template>
    <v-app>
      <v-container>
        <v-card v-if="show">
          <v-img
            :src="show.image ? show.image.original : ''"
            alt="Show Image"
            aspect-ratio="1.5"
          ></v-img>
          <v-card-title>{{ show.name }}</v-card-title>
          <v-card-subtitle>{{ show.premiered }}</v-card-subtitle>
          <v-card-text>
            <div v-html="show.summary"></div>
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="primary"
              :href="show.url"
              target="_blank"
            >
              TVMaze'de Görüntüle
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-app>
  </template>
  
  <script>
  export default {
    props: ['id'],
    data() {
      return {
        show: null,
        loading: true,
      };
    },
    
    async created() {
      this.fetchShowDetail();
    },

    methods: {
    
        async fetchShowDetail() {
        try {
          const response = await fetch(`https://api.tvmaze.com/shows/${this.id}`);
          this.show = await response.json();
        } catch (error) {
          console.error("API Error:", error);
        } finally {
          this.loading = false;
        }
      },
    },
  };
  </script>