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
