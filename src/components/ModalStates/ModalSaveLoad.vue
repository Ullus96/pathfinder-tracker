<template>
  <div class="modal__card" :style="{ top: '50%' }">
    <div class="modal__header">
      <h2>Сохранение и загрузка</h2>
      <div class="modal__close" @click="closeModal">&#10005;</div>
    </div>
    <!-- modal main content -->
    <div class="modal__content modal__scrollable">
      <!-- state selector -->
      <div class="modal__states mb-medium">
        <div
          class="modal__state-item"
          :class="[state === 1 ? 'active' : '']"
          @click="setState(1)"
        >
          Save/Load
        </div>
        <div
          class="modal__state-item"
          :class="[state === 2 ? 'active' : '']"
          @click="setState(2)"
        >
          Группа игроков
        </div>
      </div>
      <!-- end of state selector -->

      <!-- states: -->
      <!-- save/load state -->
      <template v-if="state == 1">
        <!-- input fields -->
        <div class="modal__values">
          <div class="modal__value" style="margin-bottom: 0px">
            <p>Имя:</p>
            <input
              class="modal__input"
              type="text"
              placeholder="Имя нового сохранения"
              v-model="newSaveName"
              @input="updateNewSaveName"
              @keyup.enter="addNewSaveToLocalStorage()"
              @keyup.esc="closeModal"
            />
          </div>
        </div>
        <!-- end of input -->
        <div class="modal__btns" style="margin-bottom: 0.6rem">
          <div class="btn" @click="addNewSaveToLocalStorage()">Сохранить</div>
        </div>

        <!-- available saves list -->
        <div class="modal__block mb-medium">
          <div class="modal__title">
            <h4 class="modal__title--text">Доступные сохранения</h4>
          </div>
          <div class="modal__saves-list modal__scrollable">
            <div
              class="modal__save btn"
              v-for="(save, idx) in saveNames"
              :key="save.id"
              :class="[idx === activeSave ? 'active' : '']"
              @click="setActiveSaveIdx(idx)"
            >
              <!-- content here -->
              <div class="modal__save-name">
                <div @click.stop="setMainSave(idx)">
                  <i
                    class="fa-solid fa-star text-yellow modal__save-favourite"
                    v-if="save.isMainSave"
                  ></i>
                  <i
                    class="fa-regular fa-star modal__save-not-accented-icon"
                    v-else
                  ></i>
                </div>
                <p>
                  {{ save.name }}
                </p>
              </div>
              <div class="modal__save-right-side">
                <div class="modal__save-details">
                  <div class="modal__save-date">
                    {{ save.hours }}:{{ save.minutes }}
                  </div>
                  <div class="modal__save-date modal__save-day">
                    {{ save.day }}.{{ save.month }}
                  </div>
                </div>
                <div
                  class="modal__save-delete modal__save-not-accented-icon"
                  @click.stop="deleteSave(idx)"
                >
                  <i class="fa-solid fa-trash-can"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- end of saves list -->

        <!-- btns -->
        <div class="modal__btns">
          <div class="btn btn-red" @click="rewriteSave()">Перезаписать</div>
          <div class="btn" @click="loadFromLocalStorage()">Загрузить</div>
        </div>
      </template>
      <!-- end of Save/Load state -->

      <!-- Group state: -->
      <template v-if="state == 2">
        <div class="modal__content modal__scrollable">
          <!-- group names -->
          <div class="modal__block mb-medium">
            <div class="modal__title">
              <h4 class="modal__title--text">Текущая группа:</h4>
            </div>
            <p class="modal__group-names">
              <template v-if="groupData">
                <template v-for="(char, i) in groupData" :key="i">
                  {{
                    i === groupData.length - 1
                      ? `${char.name}`
                      : `${char.name}, `
                  }}</template
                ></template
              >
              <template v-else>В вашей группе пока нет членов</template>
            </p>
          </div>
          <!-- end of names -->

          <!-- btns -->
          <div class="modal__btns">
            <div
              class="btn"
              :class="[groupData ? 'btn-red' : '']"
              @click="saveGroup()"
            >
              {{ groupData ? `Перезаписать` : `Сохранить` }}
            </div>
            <div class="btn" @click="loadGroup()" v-if="groupData">
              Добавить на поле
            </div>
          </div>
        </div>
      </template>

      <!-- End of Group state -->
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  props: ["saveNames", "activeSave", "groupData"],
  emits: [
    "closeModal",
    "addNewSaveToLocalStorage",
    "updateSaveName",
    "setActiveSaveIdx",
    "setMainSave",
    "deleteSave",
    "rewriteSave",
    "loadFromLocalStorage",
    "saveGroup",
    "loadGroup",
  ],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
    }

    function addNewSaveToLocalStorage() {
      context.emit("addNewSaveToLocalStorage");
    }

    const newSaveName = ref("");

    function updateNewSaveName() {
      context.emit("updateNewSaveName", newSaveName.value);
    }

    function setActiveSaveIdx(idx) {
      context.emit("setActiveSaveIdx", idx);
    }

    function setMainSave(idx) {
      context.emit("setMainSave", idx);
    }

    function deleteSave(idx) {
      context.emit("deleteSave", idx);
    }

    function rewriteSave() {
      context.emit("rewriteSave");
    }

    function loadFromLocalStorage() {
      context.emit("loadFromLocalStorage");
    }

    function saveGroup() {
      context.emit("saveGroup");
    }

    function loadGroup() {
      context.emit("loadGroup");
    }

    const state = ref(1);
    function setState(val) {
      state.value = val;
    }

    return {
      closeModal,
      addNewSaveToLocalStorage,
      newSaveName,
      updateNewSaveName,
      setActiveSaveIdx,
      setMainSave,
      deleteSave,
      rewriteSave,
      loadFromLocalStorage,
      saveGroup,
      loadGroup,
      state,
      setState,
    };
  },
};
</script>

<style></style>
