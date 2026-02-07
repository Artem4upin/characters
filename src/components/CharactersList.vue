<script setup>
import CharacterCard from "@/components/CharacterCard.vue";
defineProps({
  characters: {
    type: Array,
    required: true,
  },
})

defineEmits(["delete", "openEditModal"]);
</script>

<template>
    <ul class="characters-list" >
      <CharacterCard
        v-for="character in characters" :key="character.id"
        :character="character"
        @delete="$emit('delete', character.id)"
        @openEditModal="$emit('openEditModal', $event)"
      />
    </ul>
    <h1 v-if="characters.length === 0" class="characters-list__not-found">Нет персонажей</h1>
</template>

<style scoped lang="scss">
$primary-color: #339366;
$danger-color: #f64a4a;
$bg-dark: #1a1a1a;
$text-light: #eeeeee;

.characters-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  list-style: none;
  margin: 10px 0;
  padding: 0;

  &__not-found {
    display: flex;
    justify-content: center;
    margin-top: 200px;
  }
}

</style>
