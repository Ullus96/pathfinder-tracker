<template>
  <div class="modal__card">
    <div class="modal__header">
      <h2>Изменить персонажа</h2>
      <div class="modal__close" @click="closeModal">&#10005;</div>
    </div>
    <!-- modal main content -->
    <div class="modal__content modal__scrollable">
      <!-- input fields -->
      <div class="modal__values">
        <div class="modal__value">
          <p>Имя:</p>
          <input
            class="modal__input"
            type="text"
            placeholder="Название (можно пустое)"
            v-model="modalData.name"
            @keyup.enter="applyName"
            @keyup.esc="closeModal"
          />
        </div>
      </div>
      <!-- end of input -->

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
        <div class="btn" @click="applyName">Изменить</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["modalData"],
  emits: ["closeModal", "applyName"],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
    }

    function applyName() {
      context.emit("applyName");
    }

    return {
      closeModal,
      applyName,
    };
  },
};
</script>

<style></style>
