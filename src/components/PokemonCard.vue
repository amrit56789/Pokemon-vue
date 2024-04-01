<template>
<div class="card" @click="handleClick">
    <div class="card-title">
        <h2>{{ formattedTitleName }}</h2>
        <h2>#{{ pokemon.id }}</h2>
    </div>
    <div class="card-img">
        <img :src="pokemon.sprites.front_default" alt="Pokemon" />
    </div>
    <div class="card-footer">
        <h2 v-for="type in pokemon.types" :key="type.type.name">{{ formattedFooterName(type.type.name) }}</h2>
    </div>
</div>
</template>

<script>
import {
    defineComponent,
    computed
} from 'vue';

export default defineComponent({
    name: 'PokemonCard',
    props: {
        pokemon: {
            type: Object,
            required: true,
        },
    },
    setup(props, {
        emit
    }) {
        const formattedTitleName = computed(() => {
            return props.pokemon.name.charAt(0).toUpperCase() + props.pokemon.name.slice(1);
        });

        const formattedFooterName = (name) => {
            return name.charAt(0).toUpperCase() + name.slice(1);
        };

        const handleClick = () => {
            emit('cardClicked', props.pokemon);
        };

        return {
            handleClick,
            formattedTitleName,
            formattedFooterName,
        };
    },
});
</script>

<style scoped>
.card {
    border: 1px solid #EAEAEA;
    border-radius: 10px;
    width: 280px;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 0px 15px 15px 10px rgba(149, 63, 63, 0.2);
    margin: 20px;
    overflow: hidden;
}

.card-img img {
    width: 300px;
    height: 250px;
    object-fit: cover;
    margin: 30px 0;
}

.card-title,
.card-footer {
    width: 100%;
    text-align: center;
    padding: 10px 0;
}

.card-title {
    display: flex;
    justify-content: center;
    gap: 10px;
    border-bottom: 2px solid #EAEAEA;
    padding: 20px 0;
}

.card-footer {
    border-top: 2px solid #EAEAEA;
}

h2 {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 20px;
    margin: 5px 0;
}
</style>
