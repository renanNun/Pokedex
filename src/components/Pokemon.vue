<template>
    <div id="pokemon">
        <div class="card">
            <div class="card-image">
                <figure>
                    <img @mousedown="changeSprit" :src="currentImg">
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                    <div class="media-content">
                        <p class="title is-4">{{pokemon.num}} - {{name | upper}}</p>
                        <p class="subtitle is-6">{{pokemon.type}}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    created: function(){
        axios.get(this.url).then(res => {
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.num = res.data.game_indices[19].game_index;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.currentImg = this.pokemon.front;
        }).catch((err) => {
            console.log(err);
        })
    }, 
    data(){
        return{
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                num: '',
                front: '',
                back: ''
            }
        } 
    },
    props: {
        num: Number,
        name: String,
        url: String
    },
    filters: {
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    methods: {
        changeSprit: function(){
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            } else {
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    }
}
</script>

<style>
    #pokemon {
        margin-top: 2%;
    }
</style>