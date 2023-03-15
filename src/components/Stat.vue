<template>
  <!-- check if the stat block should be wide (if type is health) -->
  <div
    class="stats__item"
    :class="[
      `bg-${stat.color}`,
      stat.type === 'health' ? 'stats__item--wide' : '',
    ]"
    @click="
      openMenu();
      getMousePosition($event);
    "
  >
    <div
      class="menu"
      v-if="stat.isMenuShown"
      :style="[isClickedOnRightSide ? 'right: 0.3rem' : 'left: 0.3rem']"
    >
      <div class="menu__item">
        <div class="menu__counters">
          <div class="menu__counter menu__btn">-5</div>
          <div class="menu__counter menu__btn">-1</div>
          <div class="menu__counter menu__btn">+1</div>
          <div class="menu__counter menu__btn">+5</div>
        </div>
      </div>

      <div class="menu__item">
        <div class="menu__counters">
          <div class="menu__counter menu__btn">Min</div>
          <div class="menu__counter menu__btn">Max</div>
        </div>
      </div>

      <div class="menu__item">
        <p class="menu__btn">Изменить</p>
      </div>

      <div class="menu__item">
        <div class="menu__safe-space"></div>
        <p class="menu__btn" @click="removeStat">Удалить 💀</p>
      </div>
    </div>
    <!-- icon and name -->
    <i class="fa-solid" :class="`fa-${stat.icon}`"></i>
    <p v-if="stat.name" class="stats__item--name">{{ stat.name }}</p>

    <!-- values block. Also check if there is a value (or is it a note?) -->
    <div
      class="stats__item--value"
      v-if="stat.current || stat.type === 'toggle'"
    >
      <!-- if type is value and health -->
      <div v-if="stat.type !== 'toggle'" class="value">
        <p class="value--current">{{ stat.current }}</p>
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
  props: ["stat", "idx"],
  emits: ["removeStat", "openMenu"],
  setup(props, context) {
    const healthPercentage = computed(() => {
      return `${Math.floor((props.stat.current / props.stat.max) * 100)}%`;
    });

    // we emit a removeStat event with index from this certain stat
    function removeStat() {
      context.emit("removeStat", props.idx);
    }

    // emit current idx to set isMenuShown of this stat to true
    function openMenu() {
      context.emit("openMenu", props.idx);
    }

    // create a boolean that defines a position of menu (left or right)
    let isClickedOnRightSide = ref(false);

    function getMousePosition(e) {
      // check if mouse clicked on the right side of a page
      if (e.pageX > window.innerWidth / 2) {
        isClickedOnRightSide.value = true;
      } else {
        isClickedOnRightSide.value = false;
      }
    }

    return {
      healthPercentage,
      isClickedOnRightSide,
      removeStat,
      openMenu,
      getMousePosition,
    };
  },
};
</script>

<style></style>
