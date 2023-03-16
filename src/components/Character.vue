<template>
  <div class="person" :class="[turnIdx === i ? 'person__active' : '']">
    <!-- check if we should popup menu with some offset to not run out of screen -->
    <div
      class="menu"
      v-if="data.isMenuShown"
      :style="[
        isClickedOnRightHalf ? 'right: 0.3rem' : 'left: 0.2rem',
        isClickedOnBottom
          ? isRepositionShown
            ? 'top: -3.3rem; flex-direction: column-reverse;'
            : 'top: -9.3rem; flex-direction: column-reverse;'
          : 'top: 3.3rem',
      ]"
    >
      <!-- position menu -->
      <template v-if="isRepositionShown">
        <div class="menu__item">
          <div class="menu__counters">
            <div class="menu__counter menu__btn" @click="sortCharacter('top')">
              <i class="fa-solid fa-angles-up"></i>
            </div>
            <div class="menu__counter menu__btn" @click="sortCharacter(-1)">
              <i class="fa-solid fa-angle-up"></i>
            </div>
            <div class="menu__counter menu__btn" @click="sortCharacter(1)">
              <i class="fa-solid fa-angle-down"></i>
            </div>
            <div class="menu__counter menu__btn" @click="sortCharacter('bot')">
              <i class="fa-solid fa-angles-down"></i>
            </div>
          </div>
        </div>
      </template>
      <!-- end of position menu -->

      <!-- edit menu -->
      <template v-if="isEditMenuShown">
        <div class="menu__item">
          <p class="menu__btn" @click="editName">Изменить</p>
        </div>
        <!-- reverse position of safe space is clicked on bottom of screen -->
        <div
          class="menu__item"
          :style="[
            isClickedOnBottom
              ? 'display: flex; flex-direction: column-reverse;'
              : '',
          ]"
        >
          <div
            class="menu__safe-space"
            :style="[
              isClickedOnBottom
                ? 'border-bottom: 1px solid rgba(153, 153, 153, 0.1);'
                : '',
            ]"
          ></div>
          <p class="menu__btn" @click="removeCharacter()">Удалить 💀</p>
        </div>
      </template>
      <!-- end of edit menu -->
    </div>

    <div class="person__header">
      <div
        class="person__sort-btn"
        @click="
          openCharMenu('reposition');
          getMousePosition($event);
        "
      >
        <i class="fa-solid fa-sort"></i>
        <p class="person__position">{{ positionInArray }}</p>
      </div>
      <div class="person__title" :class="`bg-` + data.color">
        <div class="person__name">
          <i
            :class="[
              data.icon === 'note'
                ? `fa-regular fa-${data.icon}-sticky`
                : `fa-solid fa-${data.icon}`,
            ]"
            v-if="data.name"
          ></i>
          {{ data.name }}
        </div>
        <div
          class="person__edit"
          @click="
            openCharMenu('edit');
            getMousePosition($event);
          "
        >
          <i class="fa-solid fa-bars"></i>
        </div>
      </div>
    </div>

    <div class="stats">
      <!-- single stat -->
      <stat
        v-for="(stat, idx) in data.stats"
        :key="data.stats[idx]"
        :idx="idx"
        :stat="stat"
        :isMenuPositionBlocked="isMenuPositionBlocked"
        @removeStat="removeStat"
        @openStatMenu="openStatMenu"
        @changeStat="changeStat"
        @toggleStat="toggleStat"
        @blockMenuPos="blockMenuPos"
        @editStat="editStat"
      >
      </stat>
      <!-- end of single stat -->
      <!-- plus -->
      <div class="stats__item stats__item--add" @click="plusStat">
        <i class="fa-solid fa-plus"></i>
      </div>
      <!-- end of plus -->
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import Stat from "./Stat.vue";
export default {
  components: { Stat },
  props: ["data", "i", "isMenuPositionBlocked", "turnIdx"],
  emits: [
    "removeCharacter",
    "removeStat",
    "plusStat",
    "openStatMenu",
    "openCharMenu",
    "changeStat",
    "toggleStat",
    "blockMenuPos",
    "editStat",
    "editName",
    "sortCharacter",
  ],
  setup(props, context) {
    function removeCharacter() {
      context.emit("removeCharacter", props.i);
    }

    // we get statIndex from Stat.vue
    // then we emit payload that contains this character index
    // and send it with stat index that we got from Stat.vue
    function removeStat(statIdx) {
      context.emit("removeStat", { charIdx: props.i, statIdx });
    }

    // by clicking on plus, send index of current character to App
    function plusStat() {
      context.emit("plusStat", props.i);
    }

    // get index of stat and send it to App.vue
    // and add a current character index
    function openStatMenu(statIdx) {
      context.emit("openStatMenu", { charIdx: props.i, statIdx });
    }

    // define which menu is opened
    let isRepositionShown = ref(false);
    let isEditMenuShown = ref(false);

    // open char menu; send index of position
    function openCharMenu(source) {
      isRepositionShown.value = false;
      isEditMenuShown.value = false;

      if (source === "reposition") {
        isRepositionShown.value = true;
      } else {
        isEditMenuShown.value = true;
      }

      context.emit("openCharMenu", props.i);
    }

    // TODO: check how to import code from js idk, because there should be
    // some ways to prevent duplication of code
    // duplicate code from Stat.vue
    // create a boolean that defines a position of menu (left or right)
    let isClickedOnRightHalf = ref(false);
    let isClickedOnBottom = ref(false);

    function getMousePosition(e) {
      if (!props.isMenuPositionBlocked) {
        // check if mouse clicked on the right Half of a page
        if (e.clientX > window.innerWidth / 2) {
          isClickedOnRightHalf.value = true;
        } else {
          isClickedOnRightHalf.value = false;
        }

        // check if mouse clicked on bottom 80% of a page
        if (e.clientY > window.innerHeight * 0.8) {
          isClickedOnBottom.value = true;
          console.log(e);
        } else {
          isClickedOnBottom.value = false;
        }
      }
    }

    const positionInArray = computed(() => {
      return props.i + 1;
    });

    // quick menu payload with charIdx statIdx and value (-5 -1 +1 +5 min max)
    function changeStat(payload) {
      context.emit("changeStat", {
        charIdx: props.i,
        statIdx: payload[0],
        val: payload[1],
      });
    }

    // emit charIdx and statIdx to change toggle value
    function toggleStat(statIdx) {
      context.emit("toggleStat", { charIdx: props.i, statIdx });
    }

    // block menu from repositioning when working with buttons
    function blockMenuPos() {
      context.emit("blockMenuPos");
    }

    // open stat editing modal and emit charIdx and statIdx
    function editStat(statIdx) {
      context.emit("editStat", { charIdx: props.i, statIdx });
    }

    // open renaming modal and emit charIdx
    function editName() {
      context.emit("editName", props.i);
    }

    // emit charIdx and val (top, +1, -1, bot) to change position of character
    function sortCharacter(val) {
      context.emit("sortCharacter", { charIdx: props.i, val });
    }

    return {
      isClickedOnRightHalf,
      isClickedOnBottom,
      positionInArray,
      isRepositionShown,
      isEditMenuShown,
      removeCharacter,
      removeStat,
      plusStat,
      openStatMenu,
      getMousePosition,
      openCharMenu,
      changeStat,
      toggleStat,
      blockMenuPos,
      editStat,
      editName,
      sortCharacter,
    };
  },
};
</script>

<style></style>
