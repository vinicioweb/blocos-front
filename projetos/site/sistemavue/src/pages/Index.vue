<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h1>{{ name }}</h1>
      <q-img :src="url" width="250px" />
    </div>
    <i class="fa-solid fa-circle-arrow-right"></i>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite o nome do pokemon" />
      <q-btn color="purple" label="Pesquisar" @click="getPokemon" />
    </div>
    <div class="row justify-between absolute full-width container-arrows">
      <q-icon
        name="far fa-arrow-alt-circle-left"
        class="q-ml-sm cursor-pointer"
        size="50px"
        color="primary"
        style="font-size: 4rem"
        @click="getPokemon(id - 1)"
      >
        <q-tooltip> Ver anterior </q-tooltip>
      </q-icon>
      <q-icon
        name="far fa-arrow-alt-circle-right"
        class="q-mr-sm cursor-pointer"
        size="50px"
        color="primary"
        style="font-size: 4rem"
        @click="getPokemon(id + 1)"
      >
        <q-tooltip> Ver proximo </q-tooltip>
      </q-icon>
    </div>
  </q-page>
</template>

<script>
import api from "../services/api";
export default {
  name: "PageIndex",

  data() {
    return {
      name: "",
      url: "",
      id: null,
      search: "ditto",
    };
  },

  async beforeMount() {
    await this.getPokemon();
  },

  methods: {
    getPokemon(id) {
      api
        .get(id ? `/pokemon/${id}/` : `/pokemon/${this.search}/`)
        .then((response) => {
          this.id = response.data.id;
          this.name = response.data.name;
          this.search = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          this.triggerPositive();
        })

        .catch((error) => {
          this.triggerNegative();
        });
    },

    triggerPositive() {
      this.$q.notify({
        type: "positive",
        position: "top",
        message: "Pokemon encontrado",
      });
    },

    triggerNegative() {
      this.$q.notify({
        type: "negative",
        position: "top",
        message: "Pokemon não encontrado",
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.container-arrows {
}
</style>