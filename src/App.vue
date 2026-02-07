<script setup>
import {ref, watch, onMounted} from "vue";
import CharactersList from "./components/CharactersList.vue";
import ModalEditCharacter from "@/components/modal/ModalEditCharacter.vue";

const newName = ref('');
const newLvl = ref(0);
const selectedClass = ref(null);
const characters = ref([]);
const classList = ref(['Воин', 'Лучник', 'Маг']);

const isModalEditOpen = ref(false);
const editingCharacter = ref(null);

// запускается при запуске/перезагрузке
onMounted(() => {
  const savedCharacters = localStorage.getItem("characters");
  try {
    if (savedCharacters) {
      characters.value = JSON.parse(savedCharacters);
    }
  } catch (error) {
    console.log(error);
  }
});

watch(characters, (newList) => {
  localStorage.setItem("characters", JSON.stringify(newList));
}, {deep: true});

const addCharacter = () => {
  if (newName.value.length < 3 || newName.value > 100) {
    alert('Недопустимое количество символов имени')
    return;
  }
  if (newLvl.value < 0 || newLvl.value > 100) {
    alert('Недопустимое значение уровня')
    return;
  }
  if (selectedClass.value === null) selectedClass.value = 'Класс не выбран';
  characters.value.push({
    id: Date.now(),
    name: newName.value,
    lvl: newLvl.value,
    combatClass: selectedClass.value,
  });

  newName.value = '';
  newLvl.value = 0;
  selectedClass.value = null;
};

const removeCharacter = (id) => {
  characters.value = characters.value.filter(character => character.id !== id);
}

const openEditModal = (character) => {
  editingCharacter.value = {...character};
  isModalEditOpen.value = true;
}

const saveChanges = (updatedCharacter) => {
  const index = characters.value.findIndex(character => character.id === updatedCharacter.id);
  if (index >= 0) {
    characters.value.splice(index, 1, updatedCharacter);
    isModalEditOpen.value = false;
    editingCharacter.value = null;
  }
}

</script>

<template>

  <div class="modalEditCharacter">
  </div>

  <header class="header">
    <h1>Персонажи</h1>
  </header>

  <main>
    <div class="content">
      <div class="add-character">
        <input
          class="add-character__name"
          v-model="newName"
          placeholder="Введите имя персонажа"
          @keyup.enter="addCharacter"
        />
        <input
          class="add-character__lvl"
          type="number"
          v-model="newLvl"
          placeholder="Введите уровень"
          @keyup.enter="addCharacter"
        />
        <select
          v-model = "selectedClass"
          class="add-character__class"
        >
          <option :value="null" disabled>Выберите класс</option>
          <option v-for="c in classList" :key="c" :value="c">
            {{c}}
          </option>
        </select>
        <button @click="addCharacter" class="add-character__add-btn button">Добавить</button>
      </div>
    </div>
    <CharactersList
      :characters="characters"
      @delete="removeCharacter"
      @openEditModal="openEditModal"
    />
    <ModalEditCharacter
    v-if="isModalEditOpen"
    :character="editingCharacter"
    @close="isModalEditOpen = false"
    @save="saveChanges"
    />
  </main>

</template>

<style lang="scss" scoped>
$primary-color: #339366;
$danger-color: #f64a4a;
$bg-dark: #1a1a1a;
$text-light: #eeeeee;

.button {
  padding: 5px;
  margin: 0 5px ;
  background-color: $primary-color;
  color: white;
  border-radius: 4px;
  &:hover {
    background-color: #40b584;
  }
}

.add-character {
  display: flex;
  flex-direction: row;
  gap: 5px;

  &__name, &__lvl, &__class {
    padding: 8px;
    border-radius: 4px;
    border: 1px solid #444;
    background: #333;
    color: white;
  }
}

</style>
