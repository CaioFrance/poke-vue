<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ name | capitalize }}</p>
            <p class="subtitle is-6">Type: {{ pokemon.type }}</p>
          </div>
        </div>

        <div class="content">
          <button @click="mudarSprite()" class="button is-medium is-fullwidth">
            Mudar visualização
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    id: Number,
    name: String,
    url: String,
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: { type: String, front: String, back: String },
    };
  },
  filters: {
    capitalize(value) {
      return value[0].toUpperCase() + value.slice(1);
    },
  },
  methods: {
    mudarSprite() {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.front;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.back;
      }
    },
  },
  created() {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    });
  },
};
</script>

<style scoped>
#pokemon {
  margin: 5% 0;
}
</style>