<template>
  <!-- modal start -->
  <div class="modal" v-if="showModal">
    <div class="modal__cover" @click="closeModal"></div>
    <div class="modal__card">
      <div class="modal__header">
        <h2>Добавить характеристику</h2>
        <div class="modal__close" @click="closeModal">&#10005;</div>
      </div>
      <!-- modal main content -->
      <div class="modal__content">
        <!-- types start -->
        <div class="modal__title">
          <h4 class="modal__title--text">Тип</h4>
        </div>
        <div class="modal__types">
          <div
            v-for="(type, idx) in modalData.types"
            :key="idx"
            class="modal__type btn"
            :class="idx === modalData.activeType ? 'active' : ''"
            @click="
              modalData.activeType = idx;
              chooseType(idx);
            "
          >
            {{ type }}
          </div>
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
              v-model="modalData.name"
            />
          </div>
          <div class="modal__value" v-if="modalData.activeType !== 2">
            <p>Знач.:</p>
            <input
              class="modal__input"
              type="number"
              placeholder="Цифровое значение"
              v-model="modalData.current"
            />
          </div>
          <div
            class="modal__value"
            v-if="modalData.activeType == 1 || modalData.activeType == 3"
          >
            <p>Макс:</p>
            <input
              class="modal__input"
              type="number"
              placeholder="Максимальное значение"
              v-model="modalData.max"
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
            <div
              class="modal__item btn"
              v-for="(icon, idx) in modalData.icons"
              :key="idx"
              :class="idx === modalData.activeIcon ? 'active' : ''"
              @click="modalData.activeIcon = idx"
            >
              <i class="fa-solid" :class="`fa-${icon}`"></i>
            </div>
          </div>
        </div>
        <!-- end of icons -->

        <!-- color picker -->
        <div class="modal__block">
          <div class="modal__title">
            <h4 class="modal__title--text">Цвет</h4>
          </div>
          <div class="modal__flex">
            <div
              class="modal__item btn"
              v-for="(color, idx) in modalData.colors"
              :key="idx"
              :class="[
                `text-${color}`,
                idx === modalData.activeColor ? 'active' : '',
              ]"
              @click="modalData.activeColor = idx"
            >
              <i class="fa-solid fa-circle"></i>
            </div>
          </div>
        </div>
        <!-- end of color -->

        <!-- btns -->
        <div class="modal__btns">
          <div class="btn btn-red" @click="closeModal">Отмена</div>
          <div class="btn" @click="addStat">Применить</div>
        </div>
      </div>
    </div>
  </div>
  <!-- modal end -->
  <!-- menu cover -->
  <div
    class="menu__cover"
    v-if="isMenuCoverShown"
    @click="closeMenuByClickOnCover"
  ></div>
  <!------>
  <header class="header">
    <ul class="header__content">
      <li
        class="header__button"
        id="new-note"
        @click="addCharacter('Заметка', 'yellow', 'note')"
      >
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-regular fa-note-sticky"></i>
      </li>
      <li
        class="header__button"
        id="new-enemy"
        @click="addCharacter('Противник', 'red', 'ghost')"
      >
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-solid fa-ghost"></i>
      </li>
      <li
        class="header__button"
        id="new-npc"
        @click="addCharacter('Персонаж', 'blue', 'user')"
      >
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
      @editStat="editStat"
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
            isMenuShown: true,
          },
          {
            name: "DEF",
            color: "white",
            icon: "shield-halved",
            type: "value",
            current: 1,
            max: "",
            isMenuShown: false,
          },
          {
            name: "",
            color: "blue",
            icon: "dragon",
            type: "value",
            current: 1,
            max: 3,
            isMenuShown: false,
          },
          {
            name: "Reaction",
            color: "yellow",
            icon: "rotate-left",
            type: "toggle",
            current: 0,
            max: "",
            isMenuShown: false,
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
            isMenuShown: false,
          },
          {
            name: "DEF",
            color: "blue",
            icon: "shield-halved",
            type: "value",
            current: 8,
            max: "",
            isMenuShown: false,
          },
          {
            name: "Sp.Points",
            color: "red",
            icon: "hat-wizard",
            type: "value",
            current: 2,
            max: 7,
            isMenuShown: false,
          },
          {
            name: "Enraged",
            color: "green",
            icon: "hand-fist",
            type: "toggle",
            current: 1,
            max: "",
            isMenuShown: false,
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
            isMenuShown: false,
          },
        ],
      },
    ]);

    let showModal = ref(false);

    // data to fullfill the stat adding modal window
    const modalData = reactive({
      activeType: 0,
      types: ["Стат", "Мин/макс", "Вкл/выкл", "ХП-бар"],
      typesName: ["value", "value", "toggle", "health"],
      activeIcon: 0,
      icons: [
        "circle",
        "hand-fist",
        "hat-wizard",
        "dragon",
        "dice-d6",
        "dice-d20",
        "shoe-prints",
        "person-running",
        "eye",
        "skull",
        "shield-halved",
        "rotate-left",
        "heart",
        "brain",
        "book",
        "bolt",
        "triangle-exclamation",
        "diamond",
        "square",
        "fire",
      ],
      activeColor: 0,
      colors: ["blue", "red", "yellow", "green", "white"],
      name: "",
      current: "",
      max: "",
    });

    // create a new character by clicking on the top right icons
    function addCharacter(name, color, icon) {
      const generatedCharacter = {
        name: name,
        color: color,
        icon: icon,
        stats: [],
      };
      // add generated character to array of characters
      characters.push(generatedCharacter);
    }

    // create temporary variables that used to identify current editable position
    const currentEditable = reactive({
      charIdx: 0,
      statIdx: 0,
    });

    // open modal window on clicked 'plus' or 'edit' btns
    function editStat(charIdx) {
      showModal.value = true;
      currentEditable.charIdx = charIdx;
    }

    // add a new stat when all data checked and "proceed" button
    // on the modal window is clicked
    function addStat() {
      const generatedStat = {
        name: modalData.name,
        current: modalData.current,
        max: modalData.max,
        type: modalData.typesName[modalData.activeType],
        icon: modalData.icons[modalData.activeIcon],
        color: modalData.colors[modalData.activeColor],
      };

      characters[currentEditable.charIdx].stats.push(generatedStat);

      // clean all variables to make a new opened modal an empty one
      cleanModalValues();
    }

    // clean all variables to make a new opened modal an empty one
    function cleanModalValues() {
      showModal.value = false;
      modalData.name = "";
      modalData.current = "";
      modalData.max = "";
      wasSetToToggle = false;
    }

    // close modal window
    function closeModal() {
      cleanModalValues();
    }

    // we get character index from Character.vue then we remove
    // certain character from characters array
    function removeCharacter(charToRemove) {
      characters.splice(charToRemove, 1);
    }

    // we get characterIndex from Character.vue and statIdx from Stat.vue
    // that combined into payload in Character.vue
    // then we remove certain stat from render
    function removeStat(payload) {
      characters[payload.charIdx].stats.splice(payload.statIdx, 1);
    }

    // create an argument to tell us if type was switched to toggle
    let wasSetToToggle = false;

    // check if toggle was chosen, and set current value to 1
    // else if previous type was toggle, and another one was chosen
    // then remove current value from 1 to empty one
    function chooseType(idx) {
      if (idx == 2) {
        modalData.current = 1;
        wasSetToToggle = true;
      } else {
        if (modalData.current == 1 && wasSetToToggle) {
          modalData.current = "";
          wasSetToToggle = false;
        }
      }
    }

    // QUICK-MENU

    let isMenuCoverShown = ref(true);

    function closeMenuByClickOnCover() {
      characters[currentEditable.charIdx].stats[
        currentEditable.statIdx
      ].isMenuShown = false;

      isMenuCoverShown.value = false;
    }

    return {
      characters,
      showModal,
      modalData,
      isMenuCoverShown,
      addCharacter,
      addStat,
      editStat,
      closeModal,
      removeCharacter,
      removeStat,
      chooseType,
      closeMenuByClickOnCover,
    };
  },
};
</script>

<style scoped lang="scss"></style>
