<template>
  <!-- check if the stat block should be wide (if type is health) -->
  <div
    class="stats__item"
    :class="[
      `bg-${stat.color}`,
      stat.type === 'health' ? 'stats__item--wide' : '',
    ]"
    @click="
      openStatMenu();
      getMousePosition($event);
    "
  >
    <!-- check if we should popup menu with some offset to not run out of screen -->
    <div
      class="menu"
      v-if="stat.isMenuShown"
      :style="[
        isClickedOnRightHalf ? 'right: 0.3rem' : 'left: 0.3rem',
        isClickedOnBottom
          ? 'bottom: 3.3rem; flex-direction: column-reverse;'
          : 'top: 3.3rem',
      ]"
    >
      <!-- check if stat type isn't toggleable (bc it doesn't make sense) -->
      <div class="menu__item" v-if="stat.type !== 'toggle'">
        <div class="menu__counters">
          <div class="menu__counter menu__btn" @click="changeStat(-5)">-5</div>
          <div class="menu__counter menu__btn" @click="changeStat(-1)">-1</div>
          <div class="menu__counter menu__btn" @click="changeStat(1)">+1</div>
          <div class="menu__counter menu__btn" @click="changeStat(5)">+5</div>
        </div>
      </div>
      <!-- and here v-else type is toggleable -->
      <div class="menu__item" v-else>
        <div class="menu__counter menu__btn" @click="toggleStat()">
          Переключить
        </div>
      </div>

      <!-- same here; we don't need min/max value for toggleable -->
      <div class="menu__item" v-if="stat.max">
        <div class="menu__counters">
          <div class="menu__counter menu__btn" @click="changeStat('min')">
            Min
          </div>
          <div class="menu__counter menu__btn" @click="changeStat('max')">
            Max
          </div>
        </div>
      </div>

      <div class="menu__item">
        <p class="menu__btn">Изменить</p>
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
        <p class="menu__btn" @click="removeStat">Удалить 💀</p>
      </div>
    </div>
    <!-- icon and name -->
    <i class="fa-solid" :class="`fa-${stat.icon}`"></i>
    <p v-if="stat.name" class="stats__item--name">
      {{ stat.name }}
    </p>

    <!-- values block. Also check if there is a value (or is it a note?) -->
    <div
      class="stats__item--value"
      v-if="typeof stat.current == 'number' || stat.type === 'toggle'"
    >
      <!-- if type is value and health -->
      <div v-if="stat.type !== 'toggle'" class="value">
        <p class="value--current">
          {{ stat.current }}
        </p>
        <!-- check if there is max value to add a breakpoint and second value -->
        <template v-if="stat.max">
          <p class="value--breakpoint">/</p>
          <p class="value--total">{{ stat.max }}</p>
        </template>
      </div>
      <!-- if type is toggleable -->
      <div v-if="stat.type === 'toggle'" class="toggle">
        <div>
          <!-- toggleable is just a value that equal to 0 or 1 -->
          <!-- and we change its' status depends of the value -->
          <i
            class="fa-solid"
            :class="[stat.current == 1 ? 'fa-toggle-on' : 'fa-toggle-off']"
          ></i>
        </div>
      </div>
    </div>
    <!-- add healthbar if stat type is health -->
    <div v-if="stat.type === 'health'" class="health">
      &nbsp;
      <div
        class="health--current-bar"
        :style="{
          background: `linear-gradient(to right, #fff 0%, #fff ${healthPercentage}, transparent ${healthPercentage}`,
        }"
      ></div>
      <p class="health--amount">
        {{ healthPercentage }}
      </p>
    </div>
  </div>
</template>

<script>
import { computed, ref } from "vue";
export default {
  props: ["stat", "idx", "isMenuPositionBlocked"],
  emits: [
    "removeStat",
    "openStatMenu",
    "changeStat",
    "toggleStat",
    "blockMenuPos",
  ],
  setup(props, context) {
    const healthPercentage = computed(() => {
      return `${Math.floor((props.stat.current / props.stat.max) * 100)}%`;
    });

    // we emit a removeStat event with index from this certain stat
    function removeStat() {
      context.emit("removeStat", props.idx);
    }

    // emit current idx to set isMenuShown of this stat to true
    function openStatMenu() {
      context.emit("openStatMenu", props.idx);
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
      context.emit("blockMenuPos");
    }

    // emit current stat idx with value (-1 -5 1 5 min max)
    function changeStat(val) {
      context.emit("changeStat", [props.idx, val]);
    }

    function toggleStat() {
      context.emit("toggleStat", props.idx);
    }

    return {
      healthPercentage,
      isClickedOnRightHalf,
      isClickedOnBottom,
      removeStat,
      openStatMenu,
      getMousePosition,
      changeStat,
      toggleStat,
    };
  },
};
</script>

<style></style>
