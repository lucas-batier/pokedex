<script setup>
import {computed} from "vue";

const props = defineProps({
  pokemon: {
    type: Object,
    required: true
  }
})

const formattedId = computed(() => `#${String(props.pokemon.id).padStart(3, '0')}`)

function translateStat(statName) {
  const statTranslations = {
    "hp": "PV",
    "attack": "Attaque",
    "defense": "Défense",
    "special-attack": "Attaque Sp.",
    "special-defense": "Défense Sp.",
    "speed": "Vitesse",
  }
  return statTranslations[statName] || statName
}

function translateTypeToEmoji(typeName) {
  const typeTranslations = {
    fire: "🔥",
    water: "💧",
    grass: "🌿",
    electric: "⚡",
    ice: "❄️",
    fighting: "🥊",
    poison: "☠️",
    ground: "🌍",
    flying: "🌬️",
    psychic: "🔮",
    bug: "🐛",
    rock: "🪨",
    ghost: "👻",
    dragon: "🐉",
    dark: "🌑",
    steel: "🔩",
    fairy: "🧚",
    normal: "✨",
  }
  return typeTranslations[typeName] || typeName
}
</script>

<template>
  <span class="pokemon-id" :style="{color: `var(--vt-c-${pokemon.types?.[0].en})`}">{{ formattedId }}</span>
  <img :src="pokemon.image" alt="Pokemon Image" class="pokemon-image"/>
  <div class="pokemon-card">
    <div class="pokemon-main-info">
      <h1>{{ pokemon.name }}</h1>
      <div class="pokemon-info">
        <div class="pokemon-specific-info">
          <span>{{ pokemon.height }}m</span>
          <span class="pokemon-type-subtitle">Taille</span>
        </div>
        <div class="pokemon-type">
          <div v-for="type in pokemon.types" :key="type.en" class="pokemon-specific-info">
            <span class="pokemon-type-emoji">{{ translateTypeToEmoji(type.en) }}</span>
            <span class="pokemon-type-subtitle">{{ type.fr }}</span>
          </div>
        </div>
        <div class="pokemon-specific-info">
          <span>{{ pokemon.weight }}kg</span>
          <span class="pokemon-type-subtitle">Poids</span>
        </div>
      </div>
    </div>
    <div class="stats">
      <div v-for="stat in pokemon.stats" :key="stat.name" class="stat">
        <span class="stat-name">{{ translateStat(stat.name) }}</span>
        <span class="stat-value">{{ stat.value }}</span>
        <div class="progress-bar"
             :style="{flex: stat.value / 100, backgroundColor: `var(--vt-c-${pokemon.types[0].en})`}"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  line-height: 1;
}

.pokemon-id {
  font-size: 64px;
  position: relative;
}

.pokemon-image {
  width: 200px;
  height: auto;
  position: absolute;
  left: calc(50vw - 100px);
}

.pokemon-card {
  text-align: center;
  padding: 100px 32px 32px;
  background: rgba(255, 255, 255, 0.9);
}

.pokemon-info {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-top: 32px;
}

.pokemon-type {
  display: flex;
  gap: 8px;
}

.pokemon-type-emoji {
  font-size: 24px;
}

.pokemon-type-subtitle {
  font-size: 12px;
}

.pokemon-specific-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: end;
  line-height: 1.3;
}

.stats {
  display: flex;
  flex-direction: column;
  margin-top: 32px;
  gap: 8px;
}

.stat {
  display: flex;
}

.stat-name {
  min-width: 92px;
  text-align: left;
}

.stat-value {
  min-width: 40px;
  text-align: right;
}

.progress-bar {
  margin-left: 16px;
}

@media (min-width: 1024px) {
  .pokemon-id {
    flex: 1;
    font-size: 128px;
  }

  .pokemon-image {
    width: 400px;
    left: calc(50vw - 200px);
  }

  .pokemon-card {
    background: none;
    width: 100%;
    text-align: left;
    padding: 0 32px;
    display: flex;
    justify-content: space-between;
    align-items: end;
  }

  .pokemon-info {
    margin-bottom: 16px;
  }

  .pokemon-main-info {
    display: flex;
    flex-direction: column-reverse;
  }

  .stats {
    width: 300px;
  }
}
</style>
