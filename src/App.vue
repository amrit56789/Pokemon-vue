<template>
<div class="container">
    <PokemonCard v-for="pokemon in pokemons" :key="pokemon.id" :pokemon="pokemon" @cardClicked="fetchEvolutionData" />
    <div class="evolutions" v-if="evolutions.length">
        <PokemonCard v-for="evolution in evolutions" :key="evolution.id" :pokemon="evolution" @cardClicked="fetchEvolutionData" />
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

            Promise.all(urls.map(url => fetch(url).then(res => res.json())))
                .then(results => {
                    pokemons.value = results;
                })
                .catch(error => console.error("Failed to fetch Pokémon data:", error));
        });

        const fetchEvolutionData = (pokemon) => {
            const evolutionIds = [pokemon.id + 1, pokemon.id + 2];
            const urls = evolutionIds.map(id => `https://pokeapi.co/api/v2/pokemon/${id}`);

            Promise.all(urls.map(url => fetch(url).then(res => res.json())))
                .then(results => {
                    results.forEach(newEvolution => {
                        if (!evolutions.value.some(evolution => evolution.id === newEvolution.id)) {
                            evolutions.value.push(newEvolution);
                        }
                    });
                })
                .catch(error => {
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
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
}

.evolutions {
    display: flex;
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-top: 20px;
}
</style>
