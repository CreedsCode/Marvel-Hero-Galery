<template>
  <div>
    <h3>Hello detailed page</h3>
    <p>{{ this.$route.params.id }}</p>

    <ul>
      <li v-for="(item, index) in character" :key="index">
        {{ item.name }}
        {{ item.description }}
      </li>
    </ul>
  </div>
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
  mounted() {
    this.getCharacter();
  },
  methods: {
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
