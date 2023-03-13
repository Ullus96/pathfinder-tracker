<template>
  <div class="person">
    <div class="person__header">
      <div class="person__sort-btn">
        <i class="fa-solid fa-sort"></i>
      </div>
      <div class="person__title" :class="`bg-` + data[i].color">
        <div class="person__name">
          <i :class="data[i].icon"></i>
          {{ data[i].name }}
        </div>
        <div class="person__edit"><i class="fa-solid fa-bars"></i></div>
      </div>
    </div>

    <div class="stats">
      <template v-for="(stat, idx) in data[i].stats" :key="data[i].stats[idx]">
        <!-- check if stat is wide (healthbar) -->
        <div
          class="stats__item"
          :class="[
            `bg-${stat.color}`,
            stat.type === 'health' ? 'stats__item--wide' : '',
          ]"
        >
          <!-- icon and name -->
          <i :class="stat.icon"></i>
          <p v-if="stat.name" class="stats__item--name">{{ stat.name }}</p>

          <!-- values (and check if there is a max value exists) -->
          <div class="stats__item--value">
            <!-- if type is value and health -->
            <div v-if="stat.type !== 'toggle'" class="value">
              <p class="value--current">{{ stat.current }}</p>
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
                  :class="[
                    stat.current == 1 ? 'fa-toggle-on' : 'fa-toggle-off',
                  ]"
                ></i>
              </div>
            </div>
          </div>
          <!-- add healthbar if type is health -->
          <div v-if="stat.type === 'health'" class="health">
            &nbsp;
            <div
              class="health--current-bar"
              :style="{
                background: `linear-gradient(to right, #fff 0%, #fff ${Math.floor(
                  (stat.current / stat.max) * 100
                )}%, transparent ${Math.floor(
                  (stat.current / stat.max) * 100
                )}%)`,
              }"
            ></div>
            <p class="health--amount">
              {{ `${Math.floor((stat.current / stat.max) * 100)}%` }}
            </p>
          </div>
        </div>
      </template>
      <!-- plus -->
      <div class="stats__item stats__item--add">
        <i class="fa-solid fa-plus"></i>
      </div>
      <!-- end of plus -->
    </div>
  </div>
</template>

<script>
// import Stat from "./Stat.vue";
export default {
  // components: { Stat },
  props: ["data", "i"],
  setup() {
    const random = Math.random();

    return {
      random,
    };
  },
};
</script>

<style></style>
