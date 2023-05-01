<template>
  <div class="modal__card" :style="{ top: '50%' }">
    <div class="modal__header">
      <h2>Добавить состояние</h2>
      <div class="modal__close" @click="closeModal">&#10005;</div>
    </div>
    <!-- modal main content -->
    <div class="modal__content">
      <!-- conditions picker -->
      <div class="modal__block modal__scrollable">
        <div class="modal__conditions-list">
          <div
            class="modal__condition btn"
            v-for="(condition, idx) in modalData.conditions"
            :key="idx"
            @click="addCondition(idx)"
          >
            <i :class="[`fa-solid fa-${condition.icon}`]"></i>
            {{ condition.name }}
          </div>
        </div>
      </div>
      <!-- end of conditions -->

      <!-- btns -->
      <div class="modal__btns">
        <div class="btn btn-red" @click="closeModal">Отмена</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["modalData"],
  emits: ["closeModal", "addCondition"],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
    }

    function addCondition(idx) {
      context.emit("addCondition", idx);
    }

    return {
      closeModal,
      addCondition,
    };
  },
};
</script>

<style></style>
