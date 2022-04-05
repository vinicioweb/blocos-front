<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h1>{{ name }}</h1>
      <q-img :src="url"  width="250px" />
    </div>

    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite o nome do pokemon" />
      <q-btn color="purple" label="Pesquisar" @click="getPokemon" />
    </div>
    <div class="row justify-between full-width">
     <q-icon name="warning" color="warning" size="2rem" />
     <q-icon name="warning" color="warning" size="2rem" />
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
      search: "ditto",
    };
  },

  async beforeMount() {
    await this.getPokemon();
  },

  methods: {
    getPokemon() {
      api
        .get(`/pokemon/${this.search}/`)
        .then((response) => {
          this.name = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          this.triggerPositive();
        })

        .catch((error) => {
          this.triggerNegative(); 
        }); 
    },
     
     triggerPositive () {
        this.$q.notify({
          type: 'positive',
          position: "top", 
          message: 'Pokemon encontrado',
        })
      },

      triggerNegative () {
        this.$q.notify({
          type: 'negative',
          position: "top",
          message: 'Pokemon não encontrado', 
        })
      },
  },
};
</script>
