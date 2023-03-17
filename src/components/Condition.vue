<template>
  <!-- check if the stat block should be wide (if type is health) -->
  <div class="stats__item conditions__item">
    <div
      class="stats__cover"
      @click="
        openConditionMenu();
        getMousePosition($event);
      "
    ></div>
    <!-- check if we should popup menu with some offset to not run out of screen -->
    <div
      class="menu conditions__menu"
      v-if="condition.isMenuShown"
      :style="[
        isClickedOnRightHalf ? 'right: 0.3rem' : 'left: 0.3rem',
        isClickedOnBottom
          ? 'bottom: 3.3rem; flex-direction: column-reverse;'
          : 'top: 3.3rem',
      ]"
    >
      <!-- check if stat type isn't toggleable (bc it doesn't make sense) -->
      <div class="menu__item" v-if="condition.type !== 'toggle'">
        <div class="menu__counters">
          <div class="menu__counter menu__btn" @click="changeCondition(-1)">
            -1
          </div>
          <div class="menu__counter menu__btn" @click="changeCondition(1)">
            +1
          </div>
        </div>
      </div>
      <!-- and here v-else type is toggleable -->
      <div class="menu__item" v-else>
        <div class="menu__counter menu__btn" @click="toggleCondition()">
          Переключить
        </div>
      </div>

      <!-- same here; we don't need min/max value for toggleable -->
      <div class="menu__item" v-if="condition.max">
        <div class="menu__counters">
          <div class="menu__counter menu__btn" @click="changeCondition('min')">
            Min
          </div>
          <div class="menu__counter menu__btn" @click="changeCondition('max')">
            Max
          </div>
        </div>
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
        <p class="menu__btn" @click="removeCondition">Удалить 💀</p>
      </div>
    </div>
    <!-- icon and name -->
    <i class="fa-solid" :class="`fa-${condition.icon}`"></i>
    <p v-if="condition.name" class="stats__item--name">
      {{ condition.name }}
    </p>

    <!-- values block. Also check if there is a value (or is it a note?) -->
    <div
      class="stats__item--value"
      v-if="typeof condition.current == 'number' || condition.type === 'toggle'"
    >
      <!-- if type is value and health -->
      <div v-if="condition.type !== 'toggle'" class="value">
        <p class="value--current">
          {{ condition.current }}
        </p>
        <!-- check if there is max value to add a breakpoint and second value -->
        <template v-if="condition.max">
          <p class="value--breakpoint">/</p>
          <p class="value--total">{{ condition.max }}</p>
        </template>
      </div>
      <!-- if type is toggleable -->
      <div v-if="condition.type === 'toggle'" class="toggle">
        <div>
          <!-- toggleable is just a value that equal to 0 or 1 -->
          <!-- and we change its' status depends of the value -->
          <i
            class="fa-solid"
            :class="[condition.current > 0 ? 'fa-toggle-on' : 'fa-toggle-off']"
          ></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref } from "vue";
export default {
  props: ["condition", "idx", "isMenuPositionBlocked"],
  emits: [
    "removeCondition",
    "openConditionMenu",
    "changeCondition",
    "toggleCondition",
    "blockMenuPos",
  ],
  setup(props, context) {
    // we emit a removeCondition event with index from this certain Condition
    function removeCondition() {
      context.emit("removeCondition", props.idx);
    }

    // emit current idx to set isMenuShown of this stat to true
    function openConditionMenu() {
      context.emit("openConditionMenu", props.idx);
    }

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
        } else {
          isClickedOnBottom.value = false;
        }
      }
    }

    // emit current stat idx with value (-1 -5 1 5 min max)
    function changeCondition(val) {
      context.emit("changeCondition", [props.idx, val]);
    }

    function toggleCondition() {
      context.emit("toggleCondition", props.idx);
    }

    return {
      isClickedOnRightHalf,
      isClickedOnBottom,
      removeCondition,
      openConditionMenu,
      getMousePosition,
      changeCondition,
      toggleCondition,
    };
  },
};
</script>

<style></style>
