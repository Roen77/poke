<template>
  <div class="list">
      <article v-for="(pokemon,i) in pokemons" :key="'poke'+i" @click="setPokemonUrl(pokemon.url)">
          <img :src="`${imageUrl}/${pokemon.id}.png`" alt="" width="96" height="96">
             <h3>{{pokemon.name}}</h3>
      </article>
      <div id="scroll-trigger" ref="infinitescrolltrigger">
          <i class="fas fa-spinner fa-spin"></i>
      </div>
  </div>
</template>

<script>
export default {
    props:['imageUrl','apiUrl'],
    data() {
        return {
            pokemons: [],
            nextUrl:'',
            currentUrl:''
        }
    },
    methods: {
        fetchData() {
            let req= new Request(this.currentUrl);
            fetch(req)
            .then((resp)=>{
                if(resp.status === 200){
                    return resp.json();
                }
            })
            .then((data)=>{
                this.nextUrl=data.next;
                data.results.forEach(pokemon=>{
                    pokemon.id=pokemon.url.split('/')
                    .filter(part=>{
                        return !!part
                    }).pop();
                    this.pokemons.push(pokemon)
                })
            })
            .catch(err=>{
                console.log(err)
            })

        },
        scsrollTrigger(){
            const observer= new IntersectionObserver((entries)=>{
                entries.forEach(entry=>{
                    if(entry.intersectionRatio>0 && this.nextUrl){
                        this.next();
                    }
                })
            });
            observer.observe(this.$refs.infinitescrolltrigger)
        },
        next(){
            this.currentUrl=this.nextUrl;
            this.fetchData();
        },
        setPokemonUrl(url){
            this.$emit('setPkemonurl',url)
        }
    },
    created(){
         this.currentUrl=this.apiUrl;
        this.fetchData();
    },
    mounted(){
        this.scsrollTrigger();
    }

}
</script>

<style  lang="scss" scoped>
.list{
    display: grid;
    grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
    grid-gap:10px;
    width: 100%;
    max-width: 510px;

    article{
        height: 150px;
        background-color: #efefef;
        text-align: center;
        text-transform: capitalize;
        border-radius: 5px;
        cursor: pointer;
        box-shadow: 0 15px 30px rgba(0,0,0,0.5);
    }
    h3{margin: 0;}
}
#scroll-trigger{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    font-size: 2rem;
    color:#efefef;
}
</style>