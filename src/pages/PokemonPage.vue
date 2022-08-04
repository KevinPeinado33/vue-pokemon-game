<template>
    
    <h1 v-if="!pokemon">Espere por favor ...</h1>

    <template v-else>
        <h1>Quién es ese pokemon?</h1>

        <PokemonPicture 
            :show-pokemon="showPokemon"
            :pokemon-id="pokemon.id" />

        <PokemonOptions
            :pokemons="pokemonArr"
            @selection-pokemon="checkAnswer" />
    </template>


    <template v-if="showAnswer">

        <h2 class="fade-in">{{ message }}</h2>

        <button @click="newGame">
            Nuevo Juego
        </button>
    </template>

</template>

<script>

import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions'


export default {
    components: { PokemonOptions, PokemonPicture },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    mounted() {
        this.mixPokemonArray()
    },
    methods: {
        async mixPokemonArray() {
            
            this.pokemonArr = await getPokemonOptions()

            const rndInt    = Math.floor( Math.random() * 4 )

            this.pokemon    = this.pokemonArr[ rndInt ]

        },

        checkAnswer( selectedId ) {

            this.showPokemon = true
            this.showAnswer  = true

            if ( selectedId === this.pokemon.id ) {
                this.message = `¡Correcto! El pokemon es ${ this.pokemon.name }`
            } else {
                this.message = `¡Incorrecto! El pokemon era ${ this.pokemon.name }`
            }

        },

        newGame() {
            this.showPokemon = false
            this.showAnswer  = false
            this.pokemonArr  = []
            this.pokemon     = null
            this.mixPokemonArray()
        }
    }
}

</script>