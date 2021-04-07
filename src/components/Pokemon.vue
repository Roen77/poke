<template>
  <div class="container">
      <h1>Pokemons</h1>
      <pokemon-search  @setPkemonurl="setPkemonurl" :apiUrl="apiUrl" ></pokemon-search>
      <pokemon-list @setPkemonurl="setPkemonurl" :imageUrl="imageUrl" :apiUrl="apiUrl"></pokemon-list>
      <pokemon-detail v-if="showDetail" :pokemonUrl="pokemonUrl" :imageUrl="imageUrl" @closeDetail="closeDetail"></pokemon-detail>
  </div>
</template>

<script>
import PokemonDetail from './PokemonDetail.vue';
import PokemonList from './PokemonList.vue'
import PokemonSearch from './PokemonSearch.vue';
export default {
  components: { PokemonList, PokemonDetail, PokemonSearch },
    data() {
        return {
            imageUrl:"https://pokeres.bastionbot.org/images/pokemon/",
            apiUrl:'https://pokeapi.co/api/v2/pokemon/',
            pokemonUrl:'',
            showDetail:false
        }
    },
    methods: {
        setPkemonurl(url) {
            this.pokemonUrl=url;
            this.showDetail=true
        },
        closeDetail(){
            this.pokemonUrl='';
            this.showDetail=false
        }
    },

}
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Acme');

  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding: 10px;
    width: calc(100% - 20px);
    min-height: calc(100vh - 20px);
    background: radial-gradient(#156F99, #0A2E50);

    font-family: 'Acme', arial;
    font-size: 1rem;
    font-weight: normal;
  }

  h1 { color: #efefef; }
</style>