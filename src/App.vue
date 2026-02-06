<script setup>
import {ref, watch, onMounted} from "vue";

//TODO Разделить на компоненты (после реализации редактирования)

const newName = ref('');
const newLvl = ref(0);
const selectedClass = ref(null);
const characters = ref([]);
const classList = ref(['Воин', 'Лучник', 'Маг']);

// переменные модалки
const modalEditOpen = ref(false);
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

// следит за значением characters и обновляет LS при изменении
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
};

const editCharacter = (id) => {
/* TODO Написать логику изменения персонажа + модалка для его изменения, выводить через v-if="modalEditOpen"
    Либо сделать встроеным редактированием по схожей системе но без модалки, менять текст на инпуты и кнопка сохранить. */
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
        <button @click="addCharacter">Добавить</button>
      </div>
    </div>

    <div class="characters">
    <ul class="characters-list">
      <li class="characters-list__item" v-for="hero in characters" :key="hero.id">
        <span>
        {{hero.name}} (уровень {{hero.lvl}}) - {{hero.combatClass}}
        </span>
        <button class="characters-list__delete-btn delete-btn" @click="removeCharacter(hero.id)">Удалить</button>
      </li>
    </ul>
    <p v-if="characters.length === 0">Нет персонажей</p>
    </div>
  </main>

</template>

<style lang="scss" scoped>
$primary-color: #42b883;
$danger-color: #f64a4a;
$bg-dark: #1a1a1a;
$text-light: #eeeeee;

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

.characters-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  list-style: none;
  margin: 10px 0;
  padding: 0;
  &__item {
    border: 1px solid #e4e3e3;
    border-radius: 4px;
    padding: 6px;
  }
  &__delete-btn {
    padding: 5px;
    margin: 0 5px ;
    background-color: $danger-color;
    color: white;
    &:hover {
      background-color: #fb5e5e;
    }
  }
}

</style>
