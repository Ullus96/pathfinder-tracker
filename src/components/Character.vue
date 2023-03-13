<template>
  <div class="person">
    <div class="person__header">
      <div class="person__sort-btn">
        <i class="fa-solid fa-sort"></i>
      </div>
      <div class="person__title" :class="`bg-` + data.color">
        <div class="person__name">
          <i
            :class="[
              data.icon === 'note'
                ? `fa-regular fa-${data.icon}-sticky`
                : `fa-solid fa-${data.icon}`,
            ]"
          ></i>
          {{ data.name }}
        </div>
        <div class="person__edit" @click="removeCharacter">
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
        @removeStat="removeStat"
      >
      </stat>
      <!-- end of single stat -->
      <!-- plus -->
      <div class="stats__item stats__item--add">
        <i class="fa-solid fa-plus"></i>
      </div>
      <!-- end of plus -->
    </div>
  </div>
</template>

<script>
import Stat from "./Stat.vue";
export default {
  components: { Stat },
  props: ["data", "i"],
  emits: ["removeCharacter", "removeStat"],
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

    return {
      removeCharacter,
      removeStat,
    };
  },
};
</script>

<style></style>
