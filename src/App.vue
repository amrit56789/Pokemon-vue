<template>
<div class="container">
    <div class="pokemon-card">
        <PokemonCard v-for="pokemon in pokemons" :key="pokemon.id" :pokemon="pokemon" @cardClicked="fetchEvolutionData" />
    </div>
    <div class="evolutions" v-if="evolutions.length">
        <PokemonCard v-for="evolution in evolutions" :key="evolution.id" :pokemon="evolution" />
    </div>
</div>
</template>

<script>
import {
    ref,
    onMounted
} from 'vue';
import PokemonCard from './components/PokemonCard.vue';

export default {
    name: 'App',
    components: {
        PokemonCard,
    },
    setup() {
        const pokemons = ref([]);
        const evolutions = ref([]);

        onMounted(() => {
            const urls = [
                'https://pokeapi.co/api/v2/pokemon/1',
                'https://pokeapi.co/api/v2/pokemon/4',
                'https://pokeapi.co/api/v2/pokemon/7',
            ];

            Promise.all(urls.map((url) => fetch(url).then((res) => res.json())))
                .then((results) => {
                    pokemons.value = results;
                })
                .catch((error) => console.error("Failed to fetch Pokémon data:", error));
        });

        const fetchEvolutionData = (pokemon) => {
            const evolutionIds = [pokemon.id + 1, pokemon.id + 2];

            const urls = evolutionIds.map((id) => `https://pokeapi.co/api/v2/pokemon/${id}`);

            Promise.all(urls.map((url) => fetch(url).then((res) => res.json())))
                .then((results) => {
                    evolutions.value = results;
                })
                .catch((error) => {
                    console.error("Failed to fetch evolution data:", error);
                });
        };

        return {
            pokemons,
            evolutions,
            fetchEvolutionData,
        };
    },
};
</script>

<style>
.container {
    display: flex;
    flex-direction: column; 
    align-items: center; 
    justify-content: center;
    gap: 10px; 
    margin: auto;
}
.pokemon-card{
  cursor: pointer;
}
.pokemon-card, .evolutions {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}
</style>
