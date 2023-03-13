<template>
  <!-- modal start -->
  <div class="modal" style="display: none">
    <div class="modal__card">
      <div class="modal__header">
        <h2>Добавить характеристику</h2>
        <div class="modal__close">&#10005;</div>
      </div>
      <!-- modal main content -->
      <div class="modal__content">
        <!-- types start -->
        <div class="modal__title">
          <h4 class="modal__title--text">Тип</h4>
        </div>
        <div class="modal__types">
          <div class="modal__type btn">Стат</div>
          <div class="modal__type btn active">Мин/макс</div>
          <div class="modal__type btn">Вкл/выкл</div>
          <div class="modal__type btn">ХП-бар</div>
        </div>
        <!-- end of types -->

        <!-- input fields -->
        <div class="modal__values">
          <div class="modal__value">
            <p>Имя:</p>
            <input
              class="modal__input"
              type="text"
              placeholder="Название (можно пустое)"
            />
          </div>
          <div class="modal__value">
            <p>Знач.:</p>
            <input
              class="modal__input"
              type="text"
              placeholder="Цифровое значение"
            />
          </div>
          <div class="modal__value">
            <p>Макс:</p>
            <input
              class="modal__input"
              type="text"
              placeholder="Максимальное значение"
            />
          </div>
        </div>
        <!-- end of input -->

        <!-- icons -->
        <div class="modal__block">
          <div class="modal__title">
            <h4 class="modal__title--text">Иконка</h4>
          </div>
          <div class="modal__flex">
            <div class="modal__item btn">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn active">
              <i class="fa-solid fa-hand-fist"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-hat-wizard"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-dragon"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-dice-d6"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-dice-d20"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-shoe-prints"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-person-running"></i>
            </div>
            <div class="modal__item btn"><i class="fa-solid fa-eye"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-skull"></i></div>
            <div class="modal__item btn">
              <i class="fa-solid fa-shield-halved"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-rotate-left"></i>
            </div>
            <div class="modal__item btn"><i class="fa-solid fa-heart"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-brain"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-book"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-bolt"></i></div>
            <div class="modal__item btn">
              <i class="fa-solid fa-triangle-exclamation"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-diamond"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-square"></i>
            </div>
            <div class="modal__item btn"><i class="fa-solid fa-fire"></i></div>
          </div>
        </div>
        <!-- end of icons -->

        <!-- color picker -->
        <div class="modal__block">
          <div class="modal__title">
            <h4 class="modal__title--text">Цвет</h4>
          </div>
          <div class="modal__flex">
            <div class="modal__item btn text-blue">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn text-red">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn text-yellow">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn active text-green">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn text-white">
              <i class="fa-solid fa-circle"></i>
            </div>
          </div>
        </div>
        <!-- end of color -->

        <!-- btns -->
        <div class="modal__btns">
          <div class="btn btn-red">Отмена</div>
          <div class="btn">Применить</div>
        </div>
      </div>
    </div>
  </div>
  <!-- modal end -->
  <header class="header">
    <ul class="header__content">
      <li class="header__button" id="new-note">
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-regular fa-note-sticky"></i>
      </li>
      <li class="header__button" id="new-enemy">
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-solid fa-ghost"></i>
      </li>
      <li class="header__button" id="new-npc">
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-solid fa-user"></i>
      </li>
      <li class="header__button" id="toggle-menu">
        <i class="fa-regular fa-circle-question"></i>
      </li>
    </ul>
  </header>
  <section class="content">
    <character
      v-for="(data, i) in characters"
      :key="i"
      :i="i"
      :data="data"
      @removeCharacter="removeCharacter"
      @removeStat="removeStat"
    ></character>
  </section>
</template>

<script>
import Character from "./components/Character.vue";
import { ref, reactive } from "vue";
export default {
  components: { Character },
  setup() {
    const characters = reactive([
      {
        name: "Шпингалет",
        color: "blue",
        icon: "user",
        stats: [
          {
            name: "",
            color: "red",
            icon: "heart",
            type: "health",
            current: 84,
            max: 100,
          },
          {
            name: "DEF",
            color: "white",
            icon: "shield-halved",
            type: "value",
            current: 1,
            max: "",
          },
          {
            name: "",
            color: "blue",
            icon: "dragon",
            type: "value",
            current: 1,
            max: 3,
          },
          {
            name: "Reaction",
            color: "yellow",
            icon: "rotate-left",
            type: "toggle",
            current: 0,
            max: "",
          },
        ],
      },
      {
        name: "Красный Барон",
        color: "red",
        icon: "ghost",
        stats: [
          {
            name: "",
            color: "red",
            icon: "heart",
            type: "health",
            current: 103,
            max: 250,
          },
          {
            name: "DEF",
            color: "blue",
            icon: "shield-halved",
            type: "value",
            current: 8,
            max: "",
          },
          {
            name: "Sp.Points",
            color: "red",
            icon: "hat-wizard",
            type: "value",
            current: 2,
            max: 7,
          },
          {
            name: "Enraged",
            color: "green",
            icon: "hand-fist",
            type: "toggle",
            current: 1,
            max: "",
          },
        ],
      },
      {
        name: "Заметка",
        color: "yellow",
        icon: "note",
        stats: [
          {
            name: "Здания можно возводить лишь на проклятой земле",
            color: "white",
            icon: "book",
            type: "value",
            current: "",
            max: "",
          },
        ],
      },
    ]);

    function addCharacter() {}

    function addStat() {}

    // we get character index from Character.vue then we remove
    // certain character from characters array
    function removeCharacter(charToRemove) {
      characters.splice(charToRemove, 1);
    }

    // we get characterIndex from Character.vue and statIdx from Stat.vue
    // then we remove certain stat from render
    function removeStat(payload) {
      characters[payload.charIdx].stats.splice(payload.statIdx, 1);
    }

    return {
      characters,
      addCharacter,
      addStat,
      removeCharacter,
      removeStat,
    };
  },
};
</script>

<style scoped lang="scss"></style>
