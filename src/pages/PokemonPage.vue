<template lang="">
    <h1 v-if="!pokemon">Francisco un momento por favor...</h1>

    <div v-else="pokemon">
        <h1>¿Quién es este pokemón Francisco?</h1>

        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
        <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer" />
        <div v-if="showAnswer">
            <h2 class="fade-in">
                {{ message }}
            </h2>
            <button @click="newGame">
                Nuevo Juego
            </button>
        </div>
        
    </div>
    
</template>

<script>
import PokemonOptions from "../components/PokemonOptions.vue";
import PokemonPicture from "../components/PokemonPicture.vue";

import getPokemonOptions from '@/helpers/getPokemonOptions.js'

console.log(getPokemonOptions())

export default {
    name: "pokemonPage",

    components: {
        PokemonOptions,
        PokemonPicture,
    },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()

            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[ rndInt ]
        },
        checkAnswer( selectedId ) {
            this.showPokemon = true
            this.showAnswer = true

            if( selectedId === this.pokemon.id) {
                this.message = `Acertaste Francisco, ${ this.pokemon.name}`
            } else {
                this.message = `Oops, era ${ this.pokemon.name}`
            }
        },
        newGame() {
            this.showPokemon = false
            this.showAnswer  = false
            this.pokemonArr  = []
            this.pokemon     = null
            this.mixPokemonArray()
        }
    },
    mounted() {
        this.mixPokemonArray()
    }
};
</script>
<style lang=""></style>
