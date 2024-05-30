<script setup lang="ts">
import CharacterCard from './CharacterCard.vue'
</script>

<template>
  <CharacterCard>
    <template #characterPicture>
      picture
    </template>
    <template #characterName>
      Rick
    </template>
    <template #characterStatus>
      Alive
    </template>
    <template #characterLocation>
      Earth
    </template>
    <template #characterAppearance>
      The Rickshank Rickdemption
    </template>
  </CharacterCard>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
