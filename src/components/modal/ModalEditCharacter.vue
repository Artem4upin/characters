<script setup>
import {ref} from "vue";

const props = defineProps({
  character: {
    type: Object,
  },
})
const emit = defineEmits(['close', 'save'])
const errors = ref('')

const save = () => {
  try {
    if (props.character.name.length < 3) {
      throw Error('Имя не может быть короче 3 символов')
    }
    if (props.character.lvl < 0 || props.character.lvl > 100) {
      throw Error('Уровень не может быть меньше 0 и больше 100')
    }
    emit('save', props.character)
    emit('close')
  } catch (e) {
    errors.value = e.message;
  }
}
</script>

<template>
<div class="modal-background">
  <div class="modal-edit-character">
    <h3>Редактирование персонажа</h3>
    <div class="modal-edit-character__inputs">
      <label for="characterName">Имя</label>
      <input id="characterName" v-model="character.name" type="text" class="modal-edit-character__inputs__input">
      <label for="characterLvl">Уровень</label>
      <input id="characterLvl" v-model="character.lvl" type="number" class="modal-edit-character__inputs__input">
    </div>
    <span class="modal-edit-character__errors">{{errors}}</span>
    <div class="modal-edit-character__btns">
      <button @click="save" class="modal-edit-character__btns__btn">Сохранить</button>
      <button @click="$emit('close')" class="modal-edit-character__btns__btn">Отмена</button>
    </div>
  </div>
</div>
</template>

<style scoped lang="scss">
.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.modal-edit-character {
  padding: 25px;
  border-radius: 10px;
  min-width: 300px;
  border: 1px solid gray;
  background-color: white;
  &__inputs {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    &__input {

    }
  }
  &__btns {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    margin-top: 10px;
    &__btn {
      min-width: 85px;
      max-width: 100px;
    }
  }
  &__errors {
    display: flex;
    justify-content: center;
    padding: 4px;
    color: red;
  }
}
h3 {
  display: flex;
  justify-content: center;
}
</style>
