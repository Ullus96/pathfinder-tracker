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
    @click="closeStatMenuByClickOnCover"
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
      :isMenuPositionBlocked="isMenuPositionBlocked"
      @removeCharacter="removeCharacter"
      @removeStat="removeStat"
      @plusStat="plusStat"
      @openStatMenu="openStatMenu"
      @openCharMenu="openCharMenu"
      @changeStat="changeStat"
      @toggleStat="toggleStat"
      @blockMenuPos="blockMenuPos"
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
        isMenuShown: false,
        stats: [
          {
            name: "",
            color: "red",
            icon: "heart",
            type: "health",
            current: 84,
            max: 100,
            isMenuShown: false,
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
        isMenuShown: false,
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
        isMenuShown: false,
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
        isMenuShown: false,
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

    // set pointers on currently active stat
    function setPointers(payload) {
      if (payload.charIdx || payload.charIdx === 0)
        currentEditable.charIdx = payload.charIdx;
      if (payload.statIdx || payload.statIdx === 0)
        currentEditable.statIdx = payload.statIdx;
    }

    // open modal window on clicked 'plus' btn
    function plusStat(charIdx) {
      showModal.value = true;
      currentEditable.charIdx = charIdx;
    }

    // add a new stat when all data checked and "proceed" button
    // on the modal window is clicked
    function addStat() {
      const verifiedData = {
        current: "",
        max: "",
      };

      // add some verification (if health was sent with current hp but w/o max)
      if (
        modalData.typesName[modalData.activeType] === "health" &&
        !modalData.max &&
        modalData.current
      ) {
        verifiedData.max = modalData.current;
      }
      // verify for min/max w/o max was set
      else if (
        modalData.activeType === 1 &&
        !modalData.max &&
        modalData.current
      ) {
        verifiedData.max = modalData.current;
      }
      // else just proceed gained data
      else {
        verifiedData.max = modalData.max;
      }

      // same if health was passed with only current hp but w/o max
      if (
        modalData.typesName[modalData.activeType] === "health" &&
        !modalData.current &&
        modalData.max
      ) {
        verifiedData.current = modalData.max;
      }
      // here we check if min/max was checked
      else if (
        modalData.activeType === 1 &&
        !modalData.current &&
        modalData.max
      ) {
        verifiedData.current = modalData.max;
      }
      // else just proceed gained data
      else {
        verifiedData.current = modalData.current;
      }

      const generatedStat = {
        name: modalData.name,
        current: verifiedData.current,
        max: verifiedData.max,
        type: modalData.typesName[modalData.activeType],
        icon: modalData.icons[modalData.activeIcon],
        color: modalData.colors[modalData.activeColor],
        isMenuShown: false,
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
      closeMenu();
    }

    // we get characterIndex from Character.vue and statIdx from Stat.vue
    // that combined into payload in Character.vue
    // then we remove certain stat from render
    function removeStat(payload) {
      characters[payload.charIdx].stats.splice(payload.statIdx, 1);
      closeMenu();
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

    let isMenuCoverShown = ref(false);
    let isMenuPositionBlocked = ref(false);

    function blockMenuPos() {
      isMenuPositionBlocked.value = true;
    }

    function closeMenu() {
      isMenuCoverShown.value = false;
      isMenuPositionBlocked.value = false;
    }

    function closeStatMenuByClickOnCover() {
      // console.log(
      //   `Меню закрыто на: ${currentEditable.charIdx},${currentEditable.statIdx}`
      // );

      // check if stats exists, and only then set isMenuShown to false
      if (
        typeof characters[currentEditable.charIdx].stats[
          currentEditable.statIdx
        ] !== "undefined"
      ) {
        characters[currentEditable.charIdx].stats[
          currentEditable.statIdx
        ].isMenuShown = false;
      }

      characters[currentEditable.charIdx].isMenuShown = false;

      closeMenu();
    }

    function openStatMenu(payload) {
      setPointers(payload);
      // console.log(`Меню открыто на: ${payload.charIdx},${payload.statIdx}`);

      characters[payload.charIdx].stats[payload.statIdx].isMenuShown = true;

      isMenuCoverShown.value = true;
    }

    // Character Menu
    function openCharMenu(charIdx) {
      setPointers({
        charIdx: charIdx,
        statIdx: undefined,
      });

      characters[charIdx].isMenuShown = true;

      isMenuCoverShown.value = true;
      console.log(characters);
    }

    // edit stats by quick menu
    function changeStat(payload) {
      // make a shorter variable
      const editableStat = characters[payload.charIdx].stats[payload.statIdx];

      // if we get number as a payload value, then calculate (and check at 0 and max vals)
      // else if we got a 'min' or 'max', set current val as 0 or max.
      if (typeof payload.val === "number") {
        editableStat.current += payload.val;
        // block of code that doesn't allow to go past min or max values
        // removed bc GM said that players would be need minus vals on some cases
        // ---
        // if (editableStat.current < 0) editableStat.current = 0;
        // if (editableStat.max && editableStat.current > editableStat.max) {
        //   editableStat.current = editableStat.max;
        // }
      } else {
        if (payload.val === "min") {
          editableStat.current = 0;
        } else {
          editableStat.current = editableStat.max;
        }
      }
    }

    function toggleStat(payload) {
      // make a shorter variable
      const editableStat = characters[payload.charIdx].stats[payload.statIdx];
      editableStat.current === 0
        ? (editableStat.current = 1)
        : (editableStat.current = 0);
    }

    return {
      characters,
      showModal,
      modalData,
      isMenuCoverShown,
      isMenuPositionBlocked,
      addCharacter,
      addStat,
      plusStat,
      closeModal,
      removeCharacter,
      removeStat,
      chooseType,
      closeMenu,
      closeStatMenuByClickOnCover,
      openStatMenu,
      openCharMenu,
      changeStat,
      toggleStat,
      blockMenuPos,
    };
  },
};
</script>

<style scoped lang="scss"></style>
