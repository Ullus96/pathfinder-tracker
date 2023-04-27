<template>
  <!-- modal start -->
  <div class="modal" v-if="showModal" @keydown.esc="closeModal">
    <div class="modal__cover" @click="closeModal"></div>
    <!-- v-if add stat -->
    <template v-if="isStatAdding">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Добавить характеристику</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
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
                @keyup.enter="addStat"
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
                @keyup.enter="addStat"
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
                @keyup.enter="addStat"
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
            <div class="btn" @click="addStat">Применить</div>
          </div>
        </div>
      </div>
    </template>
    <!-- end of add stat -->
    <!-- v-if edit stat -->
    <template v-if="isStatEditing">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Изменить характеристику</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
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
                :value="modalCalc"
                @input="$emit('update:modalCalc', $event.target.value)"
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
    <!-- end of edit stat -->
    <!-- v-if edit node -->
    <template v-if="isNodeEditing">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Изменить заметку</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
          <!-- input fields -->
          <div class="modal__values">
            <div class="modal__value">
              <p>Имя:</p>
              <input
                class="modal__input"
                type="text"
                placeholder="Название (можно пустое)"
                v-model="modalData.name"
                @keyup.enter="applyNodeEdit"
                @keyup.esc="closeModal"
              />
            </div>
          </div>
          <!-- end of input -->

          <!-- btns -->
          <div class="modal__btns">
            <div class="btn btn-red" @click="closeModal">Отмена</div>
            <div class="btn" @click="applyNodeEdit">Изменить</div>
          </div>
        </div>
      </div>
    </template>
    <!-- end of edit note -->
    <!-- v-if edit name -->
    <template v-if="isNameEditing">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Изменить персонажа</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
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
    <!-- end of edit name -->
    <!-- start of condition modal -->
    <template v-if="isConditionAdding">
      <div class="modal__card">
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
    <!-- end of edit name -->
    <!-- v-if save-loading modal -->
    <template v-if="isSaveLoading">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Сохранение и загрузка</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
          <!-- input fields -->
          <div class="modal__values">
            <div class="modal__value" style="margin-bottom: 0px">
              <p>Имя:</p>
              <input
                class="modal__input"
                type="text"
                placeholder="Имя нового сохранения"
                :value="newSaveName"
                @input="$emit('update:newSaveName', $event.target.value)"
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
        </div>
      </div>
    </template>
    <!-- end of save-loading modal -->
    <!-- v-if save-load group -->
    <template v-if="isGroupAdding">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Добавление игровой группы</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
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
      </div>
    </template>
    <!-- end of save-loading group -->
    <!-- v-if settings -->
    <template v-if="isSettingsOpen">
      <div class="modal__card">
        <div class="modal__header">
          <h2>Настройки</h2>
          <div class="modal__close" @click="closeModal">&#10005;</div>
        </div>
        <!-- modal main content -->
        <div class="modal__content">
          <!-- settings start -->
          <div class="modal__block settings__block">
            <!-- font size -->
            <div class="settings__item" v-if="hideFromProduction">
              <p>Размер шрифта:</p>
              <div class="settings__values">
                <div class="settings__value">
                  <p>22px</p>
                </div>
                <div class="settings__controls">
                  <div class="btn settings__btn">
                    <i class="fa-solid fa-minus"></i>
                  </div>
                  <div class="btn settings__btn">
                    <i class="fa-solid fa-plus"></i>
                  </div>
                  <div class="btn settings__btn">
                    <i class="fa-solid fa-rotate-left"></i>
                  </div>
                </div>
              </div>
            </div>
            <!-- font size end -->

            <!-- saves -->
            <!-- vremenno offnuto -->
            <div class="modal__title">
              <h4 class="modal__title--text">Отладка</h4>
            </div>
            <div class="settings__item">
              <p>Удалить настройки и сохранения:</p>
              <div class="settings__values">
                <div class="settings__controls">
                  <div class="btn settings__btn" @click="clearLocalStorage()">
                    Удалить
                  </div>
                </div>
              </div>
            </div>
            <div class="settings__item">
              <p>Удалить только сохранения:</p>
              <div class="settings__values">
                <div class="settings__controls">
                  <div
                    class="btn settings__btn"
                    @click="clearSavesFromLocalStorage()"
                  >
                    Удалить
                  </div>
                </div>
              </div>
            </div>
            <div class="settings__item">
              <p>Удалить группу:</p>
              <div class="settings__values">
                <div class="settings__controls">
                  <div
                    class="btn settings__btn"
                    @click="clearGroupFromLocalStorage()"
                  >
                    Удалить
                  </div>
                </div>
              </div>
            </div>
            <!-- saves end -->
          </div>
          <!-- settings end -->

          <!-- credits; later move to other modal -->
          <div class="modal__block mb-medium">
            <div class="modal__title">
              <h4 class="modal__title--text">Авторство</h4>
            </div>
            <p class="modal__author-text">
              Оригинальная идея -<br />
              <a href="https://gamedevbram.itch.io/pathfinder-2e-combat-tracker"
                ><i class="fa-solid fa-up-right-from-square"></i> Pathfinder 2e
                Combat Tracker</a
              ><br />
              от
              <a href="https://twitter.com/gamedevbram"
                ><i class="fa-brands fa-twitter"></i>Gamedev Bram</a
              >
            </p>
            <p class="modal__author-text--sub">
              Данное веб-приложение является доработкой под свои нужды
            </p>
          </div>
          <!-- end of credits -->

          <!-- btns -->
          <div class="modal__btns">
            <div class="btn btn-red" @click="closeModal">Закрыть</div>
          </div>
        </div>
      </div>
    </template>
    <!-- end of add stat -->
  </div>
  <!-- modal end -->
</template>

<script>
export default {
  props: [
    "showModal",
    "isStatAdding",
    "isStatEditing",
    "isNodeEditing",
    "isNameEditing",
    "isConditionAdding",
    "isSaveLoading",
    "isGroupAdding",
    "isSettingsOpen",
    "modalData",
    "modalCalc",
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
    "applyNodeEdit",
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
    "update:newSaveName",
    "update:modalCalc",
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

    function applyNodeEdit() {
      context.emit("applyNodeEdit");
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

    return {
      closeModal,
      chooseType,
      addStat,
      applyEdit,
      applyNodeEdit,
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
    };
  },
};
</script>

<style></style>
