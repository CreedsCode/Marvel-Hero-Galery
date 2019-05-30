<template>
  <v-layout row>
    <v-flex xs12 sm6 offset-sm3>
      <v-card color="red">
        <v-img
          :aspect-ratio="16 / 9"
          :src="this.getImage(character[0].thumbnail.path)"
        >
        </v-img>

        <v-card-title primary-title>
          <div>
            <div class="headline white--text">{{ this.character[0].name }}</div>
            <span class="white--text">{{ this.character[0].description }}</span>
          </div>
        </v-card-title>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import { public_key } from "../marvel";
import axios from "axios";
export default {
  name: "Character",
  data() {
    return {
      character: []
    };
  },
  beforeMount() {
    this.getCharacter();
  },
  methods: {
    getImage: function(url) {
      var result = `${url}/detail.jpg`;
      // console.log(result);
      return result;
    },
    getCharacter: function() {
      var characterId = this.$route.params.id;
      axios
        .get(
          `https://gateway.marvel.com:443/v1/public/characters/${characterId}?apikey=${public_key}`
        )
        .then(result => {
          result.data.data.results.forEach(element => {
            this.character.push(element);
          });
        });
    }
  }
};
</script>
