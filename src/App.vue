<script setup>
import PokemonCard from "@/components/PokemonCard.vue";
import {onMounted, ref} from "vue";
import axios from 'axios';
import Navigation from "@/components/Navigation.vue";

const pokemon = ref({})
const currentId = ref(1)
const maxId = 151

async function fetchPokemon(id) {
  try {
    // Fetch basic Pokémon data
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
    const data = response.data

    // Fetch species data for French name
    const speciesResponse = await axios.get(`https://pokeapi.co/api/v2/pokemon-species/${id}`)
    const speciesData = speciesResponse.data

    // Extract French name and types in both French and English
    const frenchName = speciesData.names.find(name => name.language.name === "fr").name
    const translateType = (type) => {
      const typeTranslations = {
        normal: "Normal",
        fire: "Feu",
        water: "Eau",
        electric: "Électrik",
        grass: "Herbe",
        ice: "Glace",
        fighting: "Combat",
        poison: "Poison",
        ground: "Sol",
        flying: "Vol",
        psychic: "Psy",
        bug: "Insecte",
        rock: "Roche",
        ghost: "Spectre",
        dragon: "Dragon",
        dark: "Ténèbres",
        steel: "Acier",
        fairy: "Fée"
      };

      return typeTranslations[type] || type; // Returns the French translation or defaults to the original if not found
    };

    const types = data.types.map(typeInfo => ({
      en: typeInfo.type.name,
      fr: translateType(typeInfo.type.name),
    }))

    pokemon.value = {
      id: data.id,
      name: frenchName,
      image: data.sprites.other['official-artwork'].front_default,
      height: data.height / 10, // Convert to meters
      weight: data.weight / 10, // Convert to kg
      types,
      stats: data.stats.map(stat => ({
        name: stat.stat.name, // Will map to French later
        value: stat.base_stat
      }))
    }

    const setBackground = (type) => {
      const root = document.documentElement;

      root.style.setProperty('background', `var(--vt-c-gradient-${type})`);
    }

    setBackground(types[0].en);
  } catch (error) {
    console.error("Error fetching Pokémon data:", error)
  }
}

function fetchPreviousPokemon() {
  currentId.value = currentId.value > 1 ? currentId.value - 1 : maxId
  fetchPokemon(currentId.value)
}

function fetchNextPokemon() {
  currentId.value = currentId.value < maxId ? currentId.value + 1 : 1
  fetchPokemon(currentId.value)
}

onMounted(() => {
  fetchPokemon(currentId.value)
})
</script>

<template>
  <main>
    <PokemonCard :pokemon="pokemon"/>
    <Navigation @prev="fetchPreviousPokemon" @next="fetchNextPokemon"/>
  </main>
</template>

<style scoped>
</style>
