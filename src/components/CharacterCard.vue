<script setup>
defineProps({
  character: {
    type: Object,
  },
})

defineEmits(["delete", "openEditModal"]);
</script>

<template>
  <li class="character-card" >
    <h2>
      {{character.name}}
    </h2>
    <p>
      (уровень {{character.lvl}})
    </p>
    <p>
      {{character.combatClass}}
    </p>
    <button
      class="character-card__delete-btn btn delete-btn"
      @click="$emit('delete', character.id)"
    >Удалить
    </button>
    <button
      class="character-card__edit-btn btn edit-btn"
      @click="$emit('openEditModal', character)"
    >Редактировать
    </button>
  </li>
</template>

<style scoped lang="scss">
$primary-color: #339366;
$danger-color: #f64a4a;
$bg-dark: #1a1a1a;
$text-light: #eeeeee;

.character-card {
  border: 1px solid #e4e3e3;
  border-radius: 4px;
  padding: 6px;

  &__delete-btn {
    padding: 5px;
    margin: 0 5px ;
    background-color: $danger-color;
    color: white;
    border-radius: 4px;
    &:hover {
      background-color: #fb5e5e;
    }
  }
  &__edit-btn {
    padding: 5px;
    margin: 0 5px ;
    background-color: $primary-color;
    color: white;
    border-radius: 4px;
    &:hover {
      background-color: #40b584;
    }
  }
}
</style>
