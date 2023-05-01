<template>
  <div class="modal__card">
    <div class="modal__header">
      <h2>Изменить заметку</h2>
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
            @keyup.enter="applyNoteEdit"
            @keyup.esc="closeModal"
          />
        </div>
      </div>
      <!-- end of input -->

      <!-- btns -->
      <div class="modal__btns">
        <div class="btn btn-red" @click="closeModal">Отмена</div>
        <div class="btn" @click="applyNoteEdit">Изменить</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["modalData"],
  emits: ["closeModal", "applyNoteEdit"],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
    }

    function applyNoteEdit() {
      context.emit("applyNoteEdit");
    }

    return {
      closeModal,
      applyNoteEdit,
    };
  },
};
</script>

<style></style>
