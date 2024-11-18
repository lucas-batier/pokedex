<script setup>

import LeftArrow from "@/components/icons/LeftArrow.vue";
import RightArrow from "@/components/icons/RightArrow.vue";
</script>

<template>
  <div class="navigation">
    <button @click="$emit('prev')">
      <LeftArrow/>
    </button>
    <button @click="$emit('next')">
      <RightArrow/>
    </button>
  </div>
</template>

<style scoped>
.navigation {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  padding: 32px;
}

button {
  cursor: pointer;
  background: none;
  border: none;
}

@media (min-width: 1024px) {
  .navigation {
    bottom: 50vh;
  }
}
</style>