<template>
  <div class="searchbar">
      <form @submit.prevent="setPkemonurl">
          <input type="text" v-model="searchvalue">
          <i class="fas fa-search" @click="setPkemonurl"></i>
      </form>
  </div>
</template>

<script>
export default {
    props:['apiUrl'],
    data() {
        return {
            searchvalue: ""
        }
    },
    methods: {
        setPkemonurl() {
            if(this.searchvalue !==''){
                this.$emit('setPkemonurl',this.apiUrl+this.searchvalue.toLowerCase())
            }
        }
    },

}
</script>

<style lang="scss">
.searchbar{
    position: relative;
    width: 100%;
    max-width: 510px;
    background-color: #fff;
    margin-bottom: 20px;
    input{
        border: none;
        outline:none;
        padding: 10px;
        width: calc(100% - 50px);
    }
    i{
        position: absolute;
        top:10px;
        right: 10px;
        font-size: 1.25rem;
        color:#0a2e50;
        cursor: pointer;
    }
}
</style>