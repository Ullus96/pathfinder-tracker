<template>
  <!-- modal start -->
  <div class="modal" v-if="showModal" @keydown.esc="closeModal">
    <div class="modal__cover" @click="closeModal"></div>

    <!-- modal conditions -->
    <modal-stat-add
      v-if="isStatAdding"
      :modalData="modalData"
      @closeModal="closeModal"
      @chooseType="chooseType"
      @addStat="addStat"
    ></modal-stat-add>

    <modal-stat-edit
      v-if="isStatEditing"
      :modalData="modalData"
      @updateModalCalc="updateModalCalc"
      @closeModal="closeModal"
      @applyEdit="applyEdit"
    ></modal-stat-edit>

    <modal-note-edit
      v-if="isNoteEditing"
      :modalData="modalData"
      @closeModal="closeModal"
      @applyNoteEdit="applyNoteEdit"
    >
    </modal-note-edit>

    <modal-name-edit
      v-if="isNameEditing"
      :modalData="modalData"
      @closeModal="closeModal"
      @applyName="applyName"
    >
    </modal-name-edit>

    <modal-condition-add
      v-if="isConditionAdding"
      :modalData="modalData"
      @closeModal="closeModal"
      @addCondition="addCondition"
    >
    </modal-condition-add>
    <!-- end of edit name -->
    <!-- v-if save-loading modal -->
    <modal-save-load
      v-if="isSaveLoading || isGroupAdding"
      @closeModal="closeModal"
      @addNewSaveToLocalStorage="addNewSaveToLocalStorage"
      @updateNewSaveName="updateNewSaveName"
      :saveNames="saveNames"
      @setActiveSaveIdx="setActiveSaveIdx"
      :activeSave="activeSave"
      @setMainSave="setMainSave"
      @deleteSave="deleteSave"
      @rewriteSave="rewriteSave"
      @loadFromLocalStorage="loadFromLocalStorage"
      :groupData="groupData"
      @saveGroup="saveGroup"
      @loadGroup="loadGroup"
    >
    </modal-save-load>
    <!-- end of save-loading modal -->
    <!-- v-if settings -->
    <modal-settings
      v-if="isSettingsOpen"
      :hideFromProduction="hideFromProduction"
      @closeModal="closeModal"
      @clearLocalStorage="clearLocalStorage"
      @clearSavesFromLocalStorage="clearSavesFromLocalStorage"
      @clearGroupFromLocalStorage="clearGroupFromLocalStorage"
    >
    </modal-settings>
    <!-- end of settings -->
  </div>
  <!-- modal end -->
</template>

<script>
import ModalStatAdd from "./ModalStates/ModalStatAdd.vue";
import ModalStatEdit from "./ModalStates/ModalStatEdit.vue";
import ModalNoteEdit from "./ModalStates/ModalNoteEdit.vue";
import ModalNameEdit from "./ModalStates/ModalNameEdit.vue";
import ModalConditionAdd from "./ModalStates/ModalConditionAdd.vue";
import ModalSaveLoad from "./ModalStates/ModalSaveLoad.vue";
import ModalSettings from "./ModalStates/ModalSettings.vue";
export default {
  components: {
    ModalStatAdd,
    ModalStatEdit,
    ModalNoteEdit,
    ModalNameEdit,
    ModalConditionAdd,
    ModalSaveLoad,
    ModalSettings,
  },
  props: [
    "showModal",
    "isStatAdding",
    "isStatEditing",
    "isNoteEditing",
    "isNameEditing",
    "isConditionAdding",
    "isSaveLoading",
    "isGroupAdding",
    "isSettingsOpen",
    "modalData",
    "newSaveName",
    "saveNames",
    "activeSave",
    "groupData",
    "hideFromProduction",
  ],
  emits: [
    "closeModal",
    "chooseType",
    "addStat",
    "applyEdit",
    "applyNoteEdit",
    "applyName",
    "addCondition",
    "addNewSaveToLocalStorage",
    "setMainSave",
    "deleteSave",
    "setActiveSaveIdx",
    "rewriteSave",
    "loadFromLocalStorage",
    "saveGroup",
    "loadGroup",
    "clearLocalStorage",
    "clearSavesFromLocalStorage",
    "clearGroupFromLocalStorage",
    "updateNewSaveName",
    "updateModalCalc",
  ],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
    }

    function chooseType(idx) {
      context.emit("chooseType", idx);
    }

    function addStat() {
      context.emit("addStat");
    }

    function applyEdit() {
      context.emit("applyEdit");
    }

    function applyNoteEdit() {
      context.emit("applyNoteEdit");
    }

    function applyName() {
      context.emit("applyName");
    }

    function addCondition(idx) {
      context.emit("addCondition", idx);
    }

    function addNewSaveToLocalStorage() {
      context.emit("addNewSaveToLocalStorage");
    }

    function setMainSave(idx) {
      context.emit("setMainSave", idx);
    }

    function deleteSave(idx) {
      context.emit("deleteSave", idx);
    }

    function setActiveSaveIdx(idx) {
      context.emit("setActiveSaveIdx", idx);
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

    function clearLocalStorage() {
      context.emit("clearLocalStorage");
    }

    function clearSavesFromLocalStorage() {
      context.emit("clearSavesFromLocalStorage");
    }

    function clearGroupFromLocalStorage() {
      context.emit("clearGroupFromLocalStorage");
    }

    function updateModalCalc(val) {
      context.emit("updateModalCalc", val);
    }

    function updateNewSaveName(val) {
      context.emit("updateNewSaveName", val);
    }

    return {
      closeModal,
      chooseType,
      addStat,
      applyEdit,
      applyNoteEdit,
      applyName,
      addCondition,
      addNewSaveToLocalStorage,
      setMainSave,
      deleteSave,
      setActiveSaveIdx,
      rewriteSave,
      loadFromLocalStorage,
      saveGroup,
      loadGroup,
      clearLocalStorage,
      clearSavesFromLocalStorage,
      clearGroupFromLocalStorage,
      updateModalCalc,
      updateNewSaveName,
    };
  },
};
</script>

<style></style>
