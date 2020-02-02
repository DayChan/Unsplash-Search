<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
      clipped
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-view-dashboard</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Searchboard</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      clipped-left
      color="#FFFFFF"
      absolute
      :src="sourcelink[0].urls.small"
    >
      <template v-slot:img="{ props }">
          <v-img
            v-bind="props"
            gradient="to top right, rgba(255,255,255,.7), rgba(0,0,0,.7)"
          >
          </v-img>
        </template>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>UNSPLASH SEARCH</v-toolbar-title>
      <v-spacer />
      <v-row
        align="center"
        style="max-width: 650px"
      >
        <v-text-field
          :append-icon-cb="() => {}"
          placeholder="Search..."
          single-line
          color="white"
          hide-details
          solo
          v-model="searchValue"
          v-on:keyup.enter="getUpSlash"
        />
        <v-btn icon @click="getUpSlash">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-row>
    </v-app-bar>

    <v-content>
      <v-container>
        <v-row align="start">
            <v-col v-for="image in sourcelink" v-bind:key="image.urls.small" cols="3">
                <v-card @click="openLink(image.links.html)">
                  <v-img
                    :src="image.urls.small" 
                    aspect-ratio="1.5" 
                    contain
                    max-height=200
                  >
                    <template v-slot:placeholder>
                      <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                      >
                        <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                      </v-row>
                    </template>
                  </v-img>
                </v-card>
            </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy; 2020 WANG CHEN</span>
    </v-footer>
  </v-app>
</template>

<script>
  import axios from "axios"
  const ACCESS_KEY = 'a606364d9ea53466813fbd3f618b9b8c9d9ea58b2c8b5e39661c9f73a740c917'
  export default {
    props: {
      source: String,
    },
    data: () => ({
      drawer: null,
      sourcelink:{},
      searchValue: "Iceland"
    }),
    created () {
      document.title = 'UNSPLASH SEARCH';
      this.$vuetify.theme.dark = true;
      this.getUpSlash();
    },
    methods: {
      getUpSlash(){
        axios
        .get('https://api.unsplash.com/search/photos?page=1&query=' + this.searchValue, { headers: { Authorization: 'Client-ID ' + ACCESS_KEY} })
        .then(
          response => {
            this.sourcelink = response.data.results;
            this.total = response.data.total;
            this.total_pages = response.data.total_pages;
          })
      },
      openLink (url) {
        window.open(url,'_blank')
      },
      showMenu(){

      }

    }
  }
</script>