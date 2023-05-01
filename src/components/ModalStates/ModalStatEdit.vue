<template>
  <div class="modal__card">
    <div class="modal__header">
      <h2>Изменить характеристику</h2>
      <div class="modal__close" @click="closeModal">&#10005;</div>
    </div>
    <!-- modal main content -->
    <div class="modal__content modal__scrollable">
      <!-- types start -->
      <div class="modal__title">
        <h4 class="modal__title--text">Тип</h4>
      </div>
      <div class="modal__types">
        <div
          v-for="(type, idx) in modalData.types"
          :key="idx"
          class="modal__type btn"
          :class="idx === modalData.activeType ? 'active' : ''"
          @click="
            modalData.activeType = idx;
            chooseType(idx);
          "
        >
          {{ type }}
        </div>
      </div>
      <!-- end of types -->

      <!-- input fields -->
      <div class="modal__values">
        <div class="modal__value">
          <p>Имя:</p>
          <input
            class="modal__input"
            type="text"
            placeholder="Название (можно пустое)"
            v-model="modalData.name"
            @keyup.enter="applyEdit"
            @keyup.esc="closeModal"
          />
        </div>
        <div class="modal__value" v-if="modalData.activeType !== 2">
          <p>Знач.:</p>
          <input
            class="modal__input"
            type="number"
            placeholder="Цифровое значение"
            v-model="modalData.current"
            style="width: 25%"
            @keyup.enter="applyEdit"
            @keyup.esc="closeModal"
          />
          <input
            class="modal__input modal__small-placeholder"
            type="text"
            placeholder="+ - * / число (напр.+12)"
            @input="updateModalCalc"
            v-model="inputValue"
            style="width: 40%"
            @keyup.enter="applyEdit"
            @keyup.esc="closeModal"
          />
        </div>
        <div
          class="modal__value"
          v-if="modalData.activeType == 1 || modalData.activeType == 3"
        >
          <p>Макс:</p>
          <input
            class="modal__input"
            type="number"
            placeholder="Максимальное значение"
            v-model="modalData.max"
            @keyup.enter="applyEdit"
            @keyup.esc="closeModal"
          />
        </div>
      </div>
      <!-- end of input -->

      <!-- icons -->
      <div class="modal__block">
        <div class="modal__title">
          <h4 class="modal__title--text">Иконка</h4>
        </div>
        <div class="modal__flex">
          <div
            class="modal__item btn"
            v-for="(icon, idx) in modalData.icons"
            :key="idx"
            :class="idx === modalData.activeIcon ? 'active' : ''"
            @click="modalData.activeIcon = idx"
          >
            <i class="fa-solid" :class="`fa-${icon}`"></i>
          </div>
        </div>
      </div>
      <!-- end of icons -->

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
        <div class="btn" @click="applyEdit">Изменить</div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  props: ["modalData", "modalCalc"],
  emits: ["closeModal", "chooseType", "applyEdit", "updateModalCalc"],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
    }

    function chooseType(idx) {
      context.emit("chooseType", idx);
    }

    function applyEdit() {
      context.emit("applyEdit");
    }

    let inputValue = ref("");

    function updateModalCalc() {
      context.emit("updateModalCalc", inputValue.value);
    }

    return {
      closeModal,
      chooseType,
      applyEdit,
      inputValue,
      updateModalCalc,
    };
  },
};
</script>

<style></style>
