<template>
  <v-container fluid grid-list-xl>
    <v-layout row wrap text-xs-center>
      <v-flex xl1 md2 sm6 xs12 v-for="(item, index) in charactes" :key="index">
        <v-container v-if="item.type === 'ADDON'">
          <v-card
            v-if="item.id === 'RELOAD'"
            v-on:click="loadMore($event)"
            min-height="100%"
            class="red white--text"
          >
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">{{ item.name }}</h3>
              </div>
            </v-card-title>
          </v-card>

          <!-- <v-card
            v-if="item.id === 'ITEMS'"
            v-on:click="loadMore($event)"
            id="dropdown-example"
            min-height="100%"
            class="red white--text"
          >
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">{{ item.name }}</h3>
              </div>
            </v-card-title>
            <v-flex xs12 sm4>
              <p>{{ item.name }}</p>
              <v-overflow-btn
                :items="dropdown_items"
                :label="item.name"
                segmented
                target="#dropdown-example"
              ></v-overflow-btn>
            </v-flex>
          </v-card> -->
        </v-container>

        <v-card
          v-else
          min-height="100%"
          class="red white--text"
          :to="{ name: 'character', params: { id: item.id } }"
        >
          <v-img :src="getStandardImage(item.thumbnail.path)"></v-img>

          <v-card-title primary-title>
            <div>
              <h3 class="headline mb-0">{{ item.name }}</h3>
            </div>
          </v-card-title>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { public_key } from "../marvel";
import axios from "axios";

export default {
  name: "Characters",
  data() {
    return {
      charactes: [],
      nextOffset: 0
      // dropdown_items: [
      //   { text: "100%" },
      //   { text: "75%" },
      //   { text: "50%" },
      //   { text: "25%" },
      //   { text: "0%" }
      // ]
    };
  },
  mounted() {
    this.getCharacters();
  },
  methods: {
    loadMore: function(event) {
      event.target.parentElement.parentElement.parentElement.remove();
      this.getCharacters();
    },
    getStandardImage: function(url) {
      var result = `${url}/standard_fantastic.jpg`;
      // console.log(result);

      return result;
    },
    getCharacters: function() {
      axios
        .get(
          `https://gateway.marvel.com:443/v1/public/characters?offset=${
            this.nextOffset
          }&apikey=${public_key}`
        )
        // axios.get(`https://gateway.marvel.com/v1/public/characters?orderBy=name&apikey=${public_key}`)
        .then(result => {
          this.nextOffset = result.data.data.offset + 20;
          result.data.data.results.forEach(element => {
            // console.log(element);
            this.charactes.push(element);
          });
          var loadMoreElement = {
            id: "RELOAD",
            name: "Load More",
            type: "ADDON",
            thumbnail: {
              path: ""
            }
          };
          // var changeItemsPerReload = {
          //   id: "ITEMS",
          //   name: "Items get.",
          //   type: "ADDON",
          //   thumbnail: {
          //     path: ""
          //   }
          // };
          this.charactes.push(loadMoreElement);
          // this.charactes.push(changeItemsPerReload);
        });
    }
  }
};
</script>
