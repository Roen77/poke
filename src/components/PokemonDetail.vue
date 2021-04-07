<template>
  <div class="detail">
      <div class="detail-view" v-if="show">
          <div v-if="pokemon" class="image">
              <img :src="`${imageUrl}/${pokemon.id}.png`" alt="">
          </div>
          <div v-if="pokemon" class="data">
              <h2>{{pokemon.name}}</h2>
              <div class="property">
                  <div class="left">Base Experience</div>
                  <div class="right">{{pokemon.base_experience}}xp</div>
              </div>
              <div class="property">
                  <div class="left">weight</div>
                  <div class="right">{{pokemon.weight/10}}kg</div>
              </div>
              <div class="property">
                  <div class="left">height</div>
                  <div class="right">{{pokemon.height/10}}m</div>
              </div>
                <h3>Pokemon Types
                </h3>
                <div class="types">
                       <div class="type" v-for="value in pokemon.types" :key="value.slot">{{value.type.name}}</div>
                    </div>
                <h3>Abilites
                </h3>
                <div class="types">
                          <div class="type" v-for="value in pokemon.abilities" :key="value.slot">{{value.ability.name}}</div>
                </div>
          </div>
          <h3 v-else>The Pokemon was not found</h3>
          <button class="close" @click="closeDetail">close</button>
      </div>
       <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>

<script>
export default {
    props:['pokemonUrl','imageUrl'],
    data() {
        return {
            show: false,
            pokemon:{}
        }
    },
    methods: {
        closeDetail() {
            this.$emit('closeDetail')
        },
        fetchData() {
            let req= new Request(this.pokemonUrl);
            fetch(req)
            .then((resp)=>{
                if(resp.status === 200){
                    return resp.json();
                }
            })
            .then((data)=>{
                this.pokemon=data;
                this.show=true
            })
            .catch(err=>{
                console.log(err)
            })

        },
    },
    created(){
        this.fetchData();
    }

}
</script>

<style lang="scss">
.detail{
    display: flex;
    justify-content: center;
    align-items: flex-start;
    flex-direction: column;
    position: fixed;
    height: 100%;
    top:0;
    left:0;
    padding: 90px 10px 10px;
    width: calc(100% - 20px);
    background: rgba(0,0,0,0.5);

    .detail-view{
        display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: relative;
    width: 100%;
    /* max-width: 510px; */
    padding: 10px 0 0;
    background-color: #fff;
    margin: 0 auto;
    border-radius: 30px;
    }
    .image{
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 120px;
        height: 120px;
        top:-50px;
        padding: 20px;
        background-color: #333;
             border-radius: 50%;
        img{width: 100%;}
   
    }
    .data{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-direction: column;
        width: 100%;
        margin-bottom: 40px;
        .property{
            width: 90%;
            max-width: 400px;
            border-bottom: 1px solid #ddd;
        }
        .left{float: left;}
        .right{float: right;}
    }

    h2{
        padding: 0;
        margin: 0;
        text-transform: capitalize;
    }
    h3{
        width: 90%;
        max-width:400px;
        border: 1px solid #ddd;
    }
    .types{
        display: flex;
        justify-content: flex-start;
        width: 90%;
        max-width: 400px;
    }
    .type{
        margin: 0 10px 10px 0;
        padding: 7px;
        word-wrap: none;
        word-break: keep-all;
        background-color: royalblue;
        border-radius: 10px;
    }

}
.close{
    outline: none;
    border:none;
    color:#efefef;
    padding: 10px 20px;
    background-color: #333;
    cursor: pointer;
    margin-bottom: 20px;
}
</style>