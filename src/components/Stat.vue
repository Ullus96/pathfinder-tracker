<template>
  <!-- check if the stat block should be wide (if type is health) -->
  <div
    @click="removeStat"
    class="stats__item"
    :class="[
      `bg-${stat.color}`,
      stat.type === 'health' ? 'stats__item--wide' : '',
    ]"
  >
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
import { computed } from "vue";
export default {
  props: ["stat", "idx"],
  emits: ["removeStat"],
  setup(props, context) {
    const healthPercentage = computed(() => {
      return `${Math.floor((props.stat.current / props.stat.max) * 100)}%`;
    });

    // we emit a removeStat event with index from this certain stat
    function removeStat() {
      context.emit("removeStat", props.idx);
    }

    return {
      healthPercentage,
      removeStat,
    };
  },
};
</script>

<style></style>
