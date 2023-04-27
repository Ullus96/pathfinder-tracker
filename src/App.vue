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
                v-model="modalCalc"
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
                v-model="newSaveName"
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
                @click="activeSave = idx"
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

  <!-- menu cover -->
  <div
    class="menu__cover"
    v-if="isMenuCoverShown"
    @click="closeStatMenuByClickOnCover"
  ></div>
  <!------>
  <header class="header">
    <ul class="header__content">
      <div class="header__save-load" @click="openSaveModal">
        <li class="header__button header__save-load">
          <i class="fa-solid fa-floppy-disk"></i>
          &nbsp;Save/Load
        </li>
      </div>
      <div class="header__right-btns">
        <li class="header__button text-blue" @click="openGroupModal">
          <i class="fa-sharp fa-regular fa-plus"></i>
          <i class="fa-solid fa-user-group"></i>
        </li>
        <li
          class="header__button text-yellow"
          @click="addCharacter('Заметка', 'yellow', 'note')"
        >
          <i class="fa-sharp fa-regular fa-plus"></i>
          <i class="fa-regular fa-note-sticky"></i>
        </li>
        <li
          class="header__button text-red"
          @click="addCharacter('Противник', 'red', 'ghost')"
        >
          <i class="fa-sharp fa-regular fa-plus"></i>
          <i class="fa-solid fa-ghost"></i>
        </li>
        <li
          class="header__button text-blue"
          @click="addCharacter('Персонаж', 'blue', 'user')"
        >
          <i class="fa-sharp fa-regular fa-plus"></i>
          <i class="fa-solid fa-user"></i>
        </li>
        <li class="header__button text-white" @click="openSettingsModal">
          <i class="fa-solid fa-gear"></i>
        </li>
      </div>
    </ul>
  </header>
  <footer class="footer">
    <div class="footer__content">
      <div class="footer__button" @click="resetTurns()">Reset</div>
      <div class="footer__counter">
        <div class="footer__button footer__arrow" @click="makeTurn(-1)">
          <i class="fa-solid fa-angle-left"></i>
        </div>
        <div class="footer__turn">
          <p>{{ fullTurns }}</p>
        </div>
        <div class="footer__button footer__arrow" @click="makeTurn(1)">
          <i class="fa-solid fa-angle-right"></i>
        </div>
      </div>
      <div>
        <div
          class="footer__button"
          :class="[isSortingByReactionActive ? 'text-green' : '']"
          @click="sortCharacters()"
        >
          <i class="fa-solid fa-person-running"></i>
          <i class="fa-solid fa-arrow-down-wide-short"></i>
        </div>
      </div>
    </div>
  </footer>
  <section class="content">
    <character
      v-for="(data, i) in characters"
      :key="i"
      :i="i"
      :data="data"
      :isMenuPositionBlocked="isMenuPositionBlocked"
      :turnIdx="turnIdx"
      @removeCharacter="removeCharacter"
      @removeStat="removeStat"
      @plusStat="plusStat"
      @openStatMenu="openStatMenu"
      @openCharMenu="openCharMenu"
      @changeStat="changeStat"
      @toggleStat="toggleStat"
      @blockMenuPos="blockMenuPos"
      @editStat="editStat"
      @editName="editName"
      @openConditionMenu="openConditionMenu"
      @changeCondition="changeCondition"
      @toggleCondition="toggleCondition"
      @removeCondition="removeCondition"
      @plusCondition="plusCondition"
      @changeReaction="changeReaction"
      @addNote="addNote"
      @copyCharacter="copyCharacter"
      @updateNote="updateNote"
      @spoilerNote="spoilerNote"
      @editNote="editNote"
      @moveStat="moveStat"
    ></character>
  </section>
</template>

<script>
import Character from "./components/Character.vue";
import { ref, reactive, onMounted } from "vue";
export default {
  components: { Character },
  setup() {
    // data to fullfill the stat adding modal window
    const modalData = reactive({
      activeType: 0,
      types: ["Стат", "Мин/макс", "Вкл/выкл", "ХП-бар"],
      typesName: ["value", "minmax", "toggle", "health"],
      activeIcon: 0,
      icons: [
        "circle",
        "hand-fist",
        "hat-wizard",
        "dragon",
        "dice-d6",
        "dice-d20",
        "shoe-prints",
        "person-running",
        "eye",
        "skull",
        "shield-halved",
        "rotate-left",
        "heart",
        "brain",
        "book",
        "bolt",
        "triangle-exclamation",
        "diamond",
        "square",
        "fire",
      ],
      activeColor: 0,
      colors: [
        "blue",
        "red",
        "yellow",
        "green",
        "white",
        "darkblue",
        "orange",
        "gray",
        "black",
      ],
      name: "",
      current: "",
      max: "",

      conditions: [
        {
          name: "Бегство",
          nameEng: "Fleeing",
          icon: "person-running",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Без сознания",
          nameEng: "Unconscious",
          icon: "power-off",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Глухота",
          nameEng: "Deafened",
          icon: "ear-deaf",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Заворожен",
          nameEng: "Fascinated",
          icon: "face-dizzy",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Замедлен",
          nameEng: "Slowed",
          icon: "user-minus",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Замечен",
          nameEng: "Observed",
          icon: "eye",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Замешательство",
          nameEng: "Confused",
          icon: "question",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Застигнут врасплох",
          nameEng: "Flat-footed",
          icon: "shoe-prints",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Истощен",
          nameEng: "Drained",
          icon: "droplet",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Напуган",
          nameEng: "Frightened",
          icon: "face-frown-open",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Невидимый",
          nameEng: "Invisible",
          icon: "ghost",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Незамечен",
          nameEng: "Unnoticed",
          icon: "user-large-slash",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Необнаружен",
          nameEng: "Undetected",
          icon: "user-secret",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Неуклюжесть",
          nameEng: "Clumsy",
          icon: "scale-unbalanced",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Ничком",
          nameEng: "Prone",
          icon: "person-praying",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Обездвижен",
          nameEng: "Immobilized",
          icon: "link",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Обречен",
          nameEng: "Doomed",
          icon: "skull-crossbones",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Одурманен",
          nameEng: "Stupefied",
          icon: "brain",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Окаменение",
          nameEng: "Petrified",
          icon: "gem",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Ослаблен",
          nameEng: "Enfeebled",
          icon: "crutch",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Ослеплен",
          nameEng: "Dazzled",
          icon: "sun",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Ошеломлен",
          nameEng: "Stunned",
          icon: "user-xmark",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Парализован",
          nameEng: "Paralyzed",
          icon: "person",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Перегружен",
          nameEng: "Encumbered",
          icon: "weight-hanging",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Под контролем",
          nameEng: "Controlled",
          icon: "face-grin-hearts",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "При смерти",
          nameEng: "Dying",
          icon: "skull",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Ранен",
          nameEng: "Wounded",
          icon: "user-injured",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Сдерживаем",
          nameEng: "Restrained",
          icon: "user-lock",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Скрыт",
          nameEng: "Concealed",
          icon: "cloud",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Слепота",
          nameEng: "Blinded",
          icon: "eye-slash",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Спрятан",
          nameEng: "Hidden",
          icon: "mask",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Схвачен",
          nameEng: "Grabbed",
          icon: "hand-back-fist",
          type: "toggle",
          current: 1,
          max: "",
          isMenuShown: false,
        },
        {
          name: "Тошнота",
          nameEng: "Sickened",
          icon: "head-side-cough",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Ускорен",
          nameEng: "Quickened",
          icon: "user-plus",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
        {
          name: "Утомление",
          nameEng: "Fatigued",
          icon: "bed",
          type: "minmax",
          current: 1,
          max: 5,
          isMenuShown: false,
        },
      ],
    });

    let characters = reactive([]);
    // let characters = reactive([
    //   {
    //     name: "Шпингалет",
    //     color: "blue",
    //     icon: "user",
    //     isMenuShown: false,
    //     stats: [
    //       {
    //         name: "",
    //         color: "red",
    //         icon: "heart",
    //         type: "health",
    //         current: 84,
    //         max: 100,
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "DEF",
    //         color: "white",
    //         icon: "shield-halved",
    //         type: "value",
    //         current: 1,
    //         max: "",
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "",
    //         color: "blue",
    //         icon: "dragon",
    //         type: "minmax",
    //         current: 1,
    //         max: 3,
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "Reaction",
    //         color: "yellow",
    //         icon: "rotate-left",
    //         type: "toggle",
    //         current: 0,
    //         max: "",
    //         isMenuShown: false,
    //       },
    //     ],
    //     conditions: [
    //       {
    //         name: "Слепота",
    //         nameEng: "Blinded",
    //         color: "white",
    //         icon: "eye",
    //         type: "toggle",
    //         current: 1,
    //         max: "",
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "Неуклюжесть",
    //         nameEng: "Clumsy",
    //         color: "white",
    //         icon: "shoe-prints",
    //         type: "minmax",
    //         current: 1,
    //         max: 5,
    //         isMenuShown: false,
    //       },
    //     ],
    //   },
    //   {
    //     name: "Красный Барон",
    //     color: "red",
    //     icon: "ghost",
    //     isMenuShown: false,
    //     stats: [
    //       {
    //         name: "",
    //         color: "red",
    //         icon: "heart",
    //         type: "health",
    //         current: 103,
    //         max: 250,
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "DEF",
    //         color: "blue",
    //         icon: "shield-halved",
    //         type: "value",
    //         current: 8,
    //         max: "",
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "DEF",
    //         color: "blue",
    //         icon: "shield-halved",
    //         type: "value",
    //         current: 8,
    //         max: "",
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "Sp.Points",
    //         color: "red",
    //         icon: "hat-wizard",
    //         type: "minmax",
    //         current: 2,
    //         max: 7,
    //         isMenuShown: false,
    //       },
    //       {
    //         name: "Enraged",
    //         color: "green",
    //         icon: "hand-fist",
    //         type: "toggle",
    //         current: 1,
    //         max: "",
    //         isMenuShown: false,
    //       },
    //     ],
    //     conditions: [],
    //   },
    //   {
    //     name: "Заметка",
    //     color: "yellow",
    //     icon: "note",
    //     isMenuShown: false,
    //     stats: [
    //       {
    //         name: "Здания можно возводить лишь на проклятой земле",
    //         color: "white",
    //         icon: "book",
    //         type: "value",
    //         current: "",
    //         max: "",
    //         isMenuShown: false,
    //       },
    //     ],
    //     conditions: [],
    //   },
    // ]);

    let showModal = ref(false);
    let isStatAdding = ref(false);
    let isStatEditing = ref(false);
    let isNameEditing = ref(false);
    let isNodeEditing = ref(false);
    let isConditionAdding = ref(false);
    let isSaveLoading = ref(false);
    let isGroupAdding = ref(false);
    let isSettingsOpen = ref(false);

    // create a new character by clicking on the top right icons
    function addCharacter(name, color, icon) {
      const generatedCharacter = {
        name: name,
        color: color,
        icon: icon,
        reaction: 0,
        id: Date.now(),
        isMenuShown: false,
        stats: [],
        conditions: [],
      };
      // add generated character to array of characters
      characters.push(generatedCharacter);

      // if sorted by reaction, sort an new array
      if (isSortingByReactionActive.value) {
        sortByReaction();
      }
    }

    // create temporary variables that used to identify current editable position
    const currentEditable = reactive({
      charIdx: 0,
      statIdx: 0,
      condIdx: 0,
    });

    // set pointers on currently active stat
    function setPointers(payload) {
      if (payload.charIdx || payload.charIdx === 0) {
        currentEditable.charIdx = payload.charIdx;
      }
      if (payload.statIdx || payload.statIdx === 0) {
        currentEditable.statIdx = payload.statIdx;
      }
      if (payload.condIdx || payload.condIdx === 0) {
        currentEditable.condIdx = payload.condIdx;
      }
    }

    // open modal window on clicked 'plus' btn
    function plusStat(charIdx) {
      isStatAdding.value = true;
      showModal.value = true;
      currentEditable.charIdx = charIdx;
    }

    function plusCondition(charIdx) {
      isConditionAdding.value = true;
      showModal.value = true;
      currentEditable.charIdx = charIdx;

      console.log(`condition added on ${charIdx}`);
    }

    // add a new stat when all data checked and "proceed" button
    // on the modal window is clicked
    function addStat() {
      const verifiedData = {
        current: "",
        max: "",
      };

      // add some verification (if health was sent with current hp but w/o max)
      if (
        modalData.typesName[modalData.activeType] === "health" &&
        !modalData.max &&
        modalData.current
      ) {
        verifiedData.max = modalData.current;
      }
      // verify for min/max w/o max was set
      else if (
        modalData.activeType === 1 &&
        !modalData.max &&
        modalData.current
      ) {
        verifiedData.max = modalData.current;
      }
      // else just proceed gained data
      else {
        verifiedData.max = modalData.max;
      }

      // same if health was passed with only current hp but w/o max
      if (
        modalData.typesName[modalData.activeType] === "health" &&
        !modalData.current &&
        modalData.max
      ) {
        verifiedData.current = modalData.max;
      }
      // here we check if min/max was checked
      else if (
        modalData.activeType === 1 &&
        !modalData.current &&
        modalData.max
      ) {
        verifiedData.current = modalData.max;
      }
      // else just proceed gained data
      else {
        verifiedData.current = modalData.current;
      }

      if (modalData.activeType === 0) {
        verifiedData.max = "";
      }

      const generatedStat = {
        name: modalData.name,
        current: verifiedData.current,
        max: verifiedData.max,
        type: modalData.typesName[modalData.activeType],
        icon: modalData.icons[modalData.activeIcon],
        color: modalData.colors[modalData.activeColor],
        isMenuShown: false,
      };

      characters[currentEditable.charIdx].stats.push(generatedStat);

      // clean all variables to make a new opened modal an empty one
      closeModal();
      isStatAdding.value = false;
    }

    function addCondition(idx) {
      const generatedCondition = {
        name: modalData.conditions[idx].name,
        icon: modalData.conditions[idx].icon,
        type: modalData.conditions[idx].type,
        current: modalData.conditions[idx].current,
        max: modalData.conditions[idx].max,
        isMenuShown: false,
      };

      characters[currentEditable.charIdx].conditions.push(generatedCondition);
      closeModal();
    }

    function addNote(idx) {
      const generatedNote = {
        name: "Заметка",
        type: "note",
        content: "",
        height: "",
        isSpoilerOpened: true,
        isMenuShown: false,
      };

      characters[idx].stats.push(generatedNote);
      closeModal();
    }

    function copyCharacter(idx) {
      characters[idx].isMenuShown = false;
      // console.log(characters[idx]);
      const json = JSON.stringify(characters[idx]);
      let newCharacter = JSON.parse(json);
      // let newCharacter = characters[idx];
      newCharacter.id = Date.now();
      characters.push(newCharacter);
      closeMenu();
    }

    function updateNote({ charIdx, statIdx, textarea }) {
      characters[charIdx].stats[statIdx].content = textarea;
    }

    function spoilerNote({ charIdx, statIdx }) {
      characters[charIdx].stats[statIdx].isSpoilerOpened =
        !characters[charIdx].stats[statIdx].isSpoilerOpened;
    }

    // clean all variables to make a new opened modal an empty one
    function cleanModalValues() {
      // showModal.value = false;
      modalData.name = "";
      modalData.current = "";
      modalData.max = "";
      modalData.activeType = 0;
      modalData.activeIcon = 0;
      modalData.activeColor = 0;
      wasSetToToggle = false;
      modalCalc.value = "";
      activeSave.value = 0;
    }

    // close modal window
    function closeModal() {
      cleanModalValues();
      closeStatMenuByClickOnCover();
      showModal.value = false;
      isStatAdding.value = false;
      isStatEditing.value = false;
      isNameEditing.value = false;
      isNodeEditing.value = false;
      isConditionAdding.value = false;
      isSaveLoading.value = false;
      isGroupAdding.value = false;
      isSettingsOpen.value = false;
    }

    // we get character index from Character.vue then we remove
    // certain character from characters array
    function removeCharacter(charToRemove) {
      characters.splice(charToRemove, 1);
      closeMenu();
    }

    // we get characterIndex from Character.vue and statIdx from Stat.vue
    // that combined into payload in Character.vue
    // then we remove certain stat from render
    function removeStat(payload) {
      characters[payload.charIdx].stats.splice(payload.statIdx, 1);
      closeMenu();
    }

    function removeCondition(payload) {
      characters[payload.charIdx].conditions.splice(payload.condIdx, 1);
      closeMenu();
    }

    // create an argument to tell us if type was switched to toggle
    let wasSetToToggle = false;

    // check if toggle was chosen, and set current value to 1
    // else if previous type was toggle, and another one was chosen
    // then remove current value from 1 to empty one
    function chooseType(idx) {
      if (!isStatEditing && idx == 2) {
        modalData.current = 1;
        wasSetToToggle = true;
      } else {
        if (modalData.current == 1 && wasSetToToggle) {
          modalData.current = "";
          wasSetToToggle = false;
        }
      }
    }

    // QUICK-MENU

    let isMenuCoverShown = ref(false);
    let isMenuPositionBlocked = ref(false);

    function blockMenuPos() {
      isMenuPositionBlocked.value = true;
    }

    function closeMenu() {
      isMenuCoverShown.value = false;
      isMenuPositionBlocked.value = false;
    }

    function closeStatMenuByClickOnCover() {
      // console.log(
      //   `Меню закрыто на: charIdx:${currentEditable.charIdx}, statIdx:${currentEditable.statIdx}, condIdx:${currentEditable.condIdx}`
      // );

      // check if stats exists, and only then set isMenuShown to false
      if (
        characters[currentEditable.charIdx]?.stats.length > 0 &&
        typeof characters[currentEditable.charIdx].stats[
          currentEditable.statIdx
        ] !== "undefined"
      ) {
        characters[currentEditable.charIdx].stats[
          currentEditable.statIdx
        ].isMenuShown = false;
        // console.log(`statIdx is not undefined`);
      }

      // check if conditions exists
      if (
        characters[currentEditable.charIdx]?.conditions.length > 0 &&
        typeof characters[currentEditable.charIdx].conditions[
          currentEditable.condIdx
        ] !== "undefined"
      ) {
        characters[currentEditable.charIdx].conditions[
          currentEditable.condIdx
        ].isMenuShown = false;
        // console.log(`condIdx is not undefined`);
      }

      if (characters && characters.length) {
        characters[currentEditable.charIdx].isMenuShown = false;
      }

      closeMenu();
    }

    function openStatMenu(payload) {
      setPointers(payload);
      // console.log(`Меню открыто на: ${payload.charIdx},${payload.statIdx}`);

      characters[payload.charIdx].stats[payload.statIdx].isMenuShown = true;

      isMenuCoverShown.value = true;
    }

    function openConditionMenu(payload) {
      setPointers(payload);

      characters[payload.charIdx].conditions[
        payload.condIdx
      ].isMenuShown = true;
      isMenuCoverShown.value = true;
    }

    // Character Menu
    function openCharMenu(charIdx) {
      setPointers({
        charIdx: charIdx,
        statIdx: undefined,
      });

      characters[charIdx].isMenuShown = true;

      isMenuCoverShown.value = true;
    }

    // edit stats by quick menu
    function changeStat(payload) {
      // make a shorter variable
      const editableStat = characters[payload.charIdx].stats[payload.statIdx];

      // if we get number as a payload value, then calculate (and check at 0 and max vals)
      // else if we got a 'min' or 'max', set current val as 0 or max.
      if (typeof payload.val === "number") {
        editableStat.current += payload.val;

        // block of code that doesn't allow to go past min or max values
        // removed bc GM said that players would be need minus vals on some cases
        // ---
        // if (editableStat.current < 0) editableStat.current = 0;
        // if (editableStat.max && editableStat.current > editableStat.max) {
        //   editableStat.current = editableStat.max;
        // }
      } else {
        if (payload.val === "min") {
          editableStat.current = 0;
        } else {
          editableStat.current = editableStat.max;
        }
      }
    }

    function changeCondition(payload) {
      // make a shorter variable
      const editableCondition =
        characters[payload.charIdx].conditions[payload.condIdx];

      if (typeof payload.val === "number") {
        editableCondition.current += payload.val;
      } else {
        if (payload.val === "min") {
          editableCondition.current = 0;
        } else {
          editableCondition.current = editableCondition.max;
        }
      }
    }

    function toggleStat(payload) {
      // make a shorter variable
      const editableStat = characters[payload.charIdx].stats[payload.statIdx];
      editableStat.current === 0
        ? (editableStat.current = 1)
        : (editableStat.current = 0);

      editableStat.isMenuShown = false;
      closeStatMenuByClickOnCover();
    }

    function toggleCondition(payload) {
      // make a shorter variable
      const editableCondition =
        characters[payload.charIdx].conditions[payload.condIdx];
      editableCondition.current === 0
        ? (editableCondition.current = 1)
        : (editableCondition.current = 0);

      editableCondition.isMenuShown = false;
      closeStatMenuByClickOnCover();
    }

    function editStat(payload) {
      setPointers(payload);
      const editableStat = characters[payload.charIdx].stats[payload.statIdx];

      closeStatMenuByClickOnCover();
      cleanModalValues();
      showModal.value = true;
      isStatEditing.value = true;

      // set values depends on stat
      modalData.name = editableStat.name;
      modalData.current = editableStat.current;
      modalData.max = editableStat.max;

      // find index of stat's type and set it to activeType index
      modalData.activeType = modalData.typesName.findIndex(
        (el) => el === editableStat.type
      );

      // find index of stat's icon and set it to activeIcon index
      modalData.activeIcon = modalData.icons.findIndex(
        (el) => el === editableStat.icon
      );

      // find index of stat's color and set it to activeColor index
      modalData.activeColor = modalData.colors.findIndex(
        (el) => el === editableStat.color
      );
    }

    const modalCalc = ref("");
    const calcSymbols = ["-", "+", "*", "/"];

    function applyEdit() {
      const editableStat =
        characters[currentEditable.charIdx].stats[currentEditable.statIdx];

      const verifiedData = {
        current: "",
        max: "",
      };

      // add some verification (if health was sent with current hp but w/o max)
      if (
        modalData.typesName[modalData.activeType] === "health" &&
        !modalData.max &&
        modalData.current
      ) {
        verifiedData.max = modalData.current;
      }
      // verify for min/max w/o max was set
      else if (
        modalData.activeType === 1 &&
        !modalData.max &&
        modalData.current
      ) {
        verifiedData.max = modalData.current;
      }
      // else just proceed gained data
      else {
        verifiedData.max = modalData.max;
      }

      // same if health was passed with only current hp but w/o max
      if (
        modalData.typesName[modalData.activeType] === "health" &&
        !modalData.current &&
        modalData.max
      ) {
        verifiedData.current = modalData.max;
      }
      // here we check if min/max was checked
      else if (
        modalData.activeType === 1 &&
        !modalData.current &&
        modalData.max
      ) {
        verifiedData.current = modalData.max;
      }
      // Calculator functionality:
      // check if calc input contains one of symbols + - * / at 1st pos
      else if (
        modalCalc.value.length > 0 &&
        typeof modalCalc.value !== "undefined" &&
        calcSymbols.some((el) => modalCalc.value.split("").at(0).includes(el))
      ) {
        const symb = modalCalc.value.split("").at(0);
        const calcValue = +modalCalc.value.substring(1);
        if (symb === "+") {
          verifiedData.current = modalData.current + calcValue;
        } else if (symb === "-") {
          verifiedData.current = modalData.current - calcValue;
        } else if (symb === "/") {
          verifiedData.current = modalData.current / calcValue;
        } else if (symb === "*") {
          verifiedData.current = modalData.current * calcValue;
        }
      }
      // else just proceed gained data
      else {
        verifiedData.current = modalData.current;
      }

      if (modalData.activeType === 0) {
        verifiedData.max = "";
      }

      editableStat.name = modalData.name;
      editableStat.current = verifiedData.current;
      editableStat.max = verifiedData.max;
      editableStat.name = modalData.name;
      editableStat.type = modalData.typesName[modalData.activeType];
      editableStat.icon = modalData.icons[modalData.activeIcon];
      editableStat.color = modalData.colors[modalData.activeColor];
      editableStat.isMenuShown = false;

      closeModal();
      isStatEditing.value = false;
    }

    function editName(charIdx) {
      setPointers({ charIdx: charIdx });
      const editableName = characters[charIdx];

      closeStatMenuByClickOnCover();
      cleanModalValues();
      showModal.value = true;
      isNameEditing.value = true;

      // set values depends on stat
      modalData.name = editableName.name;

      // find index of stat's color and set it to activeColor index
      modalData.activeColor = modalData.colors.findIndex(
        (el) => el === editableName.color
      );
    }

    function applyName() {
      const editableName = characters[currentEditable.charIdx];

      editableName.name = modalData.name;
      editableName.color = modalData.colors[modalData.activeColor];
      editableName.isMenuShown = false;

      closeModal();
      isNameEditing.value = false;
    }

    function editNote({ charIdx, statIdx }) {
      setPointers({ charIdx, statIdx });
      const editableNote = characters[charIdx].stats[statIdx];

      closeStatMenuByClickOnCover();
      cleanModalValues();
      showModal.value = true;
      isNodeEditing.value = true;

      // set values depends on stat
      modalData.name = editableNote.name;
    }

    function applyNodeEdit() {
      const editableNote =
        characters[currentEditable.charIdx].stats[currentEditable.statIdx];

      editableNote.name = modalData.name;
      editableNote.isMenuShown = false;

      closeModal();
      isNodeEditing.value = false;
    }

    // Reaction
    let isSortingByReactionActive = ref(false);

    function changeReaction(payload) {
      setPointers(payload.charIdx);
      const editableChar = characters[currentEditable.charIdx];
      editableChar.reaction += payload.val;
    }

    function sortByReaction() {
      characters = characters.sort((a, b) => b.reaction - a.reaction);
    }

    function sortById() {
      characters = characters.sort((a, b) => a.id - b.id);
    }

    function sortCharacters() {
      if (isSortingByReactionActive.value) {
        sortById();
        isSortingByReactionActive.value = false;
      } else {
        sortByReaction();
        isSortingByReactionActive.value = true;
      }
    }

    // Stats reposition
    function moveStat({ charIdx, statIdx, val }) {
      const movableStat = characters[charIdx].stats[statIdx];
      let targetIdx;

      // if we got a -1 or +1 val (instead of "min", "max")
      if (typeof val === "number") {
        targetIdx = statIdx + val;
        // check border values
        if (targetIdx < 0) {
          targetIdx = 0;
        } else if (targetIdx > characters[charIdx].stats.length - 1) {
          targetIdx = characters[charIdx].stats.length - 1;
        }
      } else {
        val === "top"
          ? (targetIdx = 0)
          : (targetIdx = characters[charIdx].stats.length - 1);
      }

      // close menu with its' cover
      movableStat.isMenuShown = false;
      closeStatMenuByClickOnCover();
      // delete movable stat from the stats array
      characters[charIdx].stats.splice(statIdx, 1);
      // paste the movable stat at the target index with 0 deleted elements
      characters[charIdx].stats.splice(targetIdx, 0, movableStat);
    }

    // Turn counter
    let fullTurns = ref(0);
    let turnIdx = ref(-1);

    function makeTurn(val) {
      const borderIdx = characters.length - 1;
      if (turnIdx.value + val > borderIdx) {
        turnIdx.value = 0;
        fullTurns.value += 1;
      } else if (turnIdx.value + val < 0) {
        turnIdx.value = characters.length - 1;
        fullTurns.value -= 1;
      } else {
        turnIdx.value += val;
      }

      // to prevent turns counter going below 0
      if (fullTurns.value < 0) {
        fullTurns.value = 0;
        turnIdx.value = -1;
      }

      // console.log(turnIdx.value);
    }

    function resetTurns() {
      fullTurns.value = 0;
      turnIdx.value = -1;
    }

    // Save/Loading
    let saveNames = reactive([]); // data to render saves list
    let activeSave = ref(0); // what save is active on modal
    let mainSave = ref(0); // set save as main to load on start
    let newSaveName = ref("");
    let groupData = reactive(["smth"]);

    function openSaveModal() {
      showModal.value = true;
      isSaveLoading.value = true;
      getLocalStorageInfo();

      // console.log(`save names is:`);
      // console.log(saveNames);
    }

    function clearLocalStorage() {
      localStorage.clear();
    }

    onMounted(() => {
      if (localStorage.getItem("saves")) {
        try {
          // load first entity
          let gainedData = JSON.parse(localStorage.getItem("saves"));

          // gain position of main save
          gainedData.forEach((el, i) => {
            if (el.isMainSave) {
              mainSave.value = i;
            }
          });

          // load mainSave
          if (
            gainedData &&
            gainedData.length > 0 &&
            mainSave.value < gainedData.length
          ) {
            gainedData[mainSave.value].saveData.forEach((el) => {
              characters.push(el);
            });
          } else {
            gainedData[0].saveData.forEach((el) => {
              characters.push(el);
            });
          }
        } catch (e) {
          // localStorage.removeItem("characters");
          console.log(e);
        }
      }

      // if (localStorage.getItem("group")) {
      //   try {
      //     groupData = JSON.parse(localStorage.getItem("group"));
      //     console.log(groupData);
      //   } catch (e) {
      //     console.log(e);
      //   }
      // }
    });

    // Create a new save to a localStorage from scratch
    function addNewSaveToLocalStorage() {
      let storedData = [];
      if (localStorage.getItem("saves")) {
        storedData = JSON.parse(localStorage.getItem("saves"));
      }

      let generatedSave = {
        id: Date.now(),
        saveTime: Date.now(),
        name: "",
        saveData: characters,
        isMainSave: false,
      };

      if (newSaveName.value) {
        generatedSave.name = newSaveName.value;
      } else {
        generatedSave.name = "Сохранение";
      }

      // IDK why this isn't working
      // const dateFormat = new Date(generatedSave.id);
      // let keyData = {
      //   name: generatedSave.name,
      //   isMainSave: generatedSave.isMainSave,
      //   id: generatedSave.id,
      //   day: getZero(dateFormat.getDate()),
      //   month: getZero(dateFormat.getMonth() + 1),
      //   hours: getZero(dateFormat.getHours()),
      //   minutes: getZero(dateFormat.getMinutes()),
      // };

      // saveNames.push(keyData);

      // add to array of all saves a new generated save
      storedData.push(generatedSave);
      const parsed = JSON.stringify(storedData);
      localStorage.setItem("saves", parsed);

      newSaveName.value = "";
      closeModal();
    }

    function rewriteSave() {
      let storedData = JSON.parse(localStorage.getItem("saves"));

      // find save in array of saves and change saveTime and saveData
      storedData.forEach((el) => {
        if (el.id === saveNames[activeSave.value].id) {
          el.saveTime = Date.now();
          el.saveData = characters;
        }
      });

      // save a rewrited save (and all others) to a localStorage
      const parsed = JSON.stringify(storedData);
      localStorage.setItem("saves", parsed);
      closeModal();
    }

    function setMainSave(idx) {
      let storedData = JSON.parse(localStorage.getItem("saves"));

      storedData.forEach((el) => {
        el.isMainSave = false;
        if (el.id === saveNames[idx].id) {
          el.isMainSave = true;
        }
      });

      // save a rewrited save (and all others) to a localStorage
      const parsed = JSON.stringify(storedData);
      localStorage.setItem("saves", parsed);
      closeModal();
    }

    // load a save from a save list (by comparing by id)
    function loadFromLocalStorage() {
      if (localStorage.getItem("saves")) {
        try {
          if (characters) {
            characters.splice(0, characters.length);
          }
          let gainedData = JSON.parse(localStorage.getItem("saves"));

          gainedData.forEach((el) => {
            // console.log(saveNames[activeSave.value]);
            if (el.id === saveNames[activeSave.value].id) {
              el.saveData.forEach((char) => {
                characters.push(char);
              });
            }
          });
        } catch (e) {
          // localStorage.removeItem("characters");
          console.log(e);
        }
      }
      closeModal();
    }

    // AFAIK, localStorage is not reactive, so we get all render data on first load of page
    function getLocalStorageInfo() {
      saveNames = [];
      const gainedData = JSON.parse(localStorage.getItem("saves"));

      gainedData?.forEach((el) => {
        const dateFormat = new Date(el.saveTime);
        let keyData = {
          name: el.name,
          isMainSave: el.isMainSave,
          id: el.id,
          day: getZero(dateFormat.getDate()),
          month: getZero(dateFormat.getMonth() + 1),
          hours: getZero(dateFormat.getHours()),
          minutes: getZero(dateFormat.getMinutes()),
        };

        saveNames.push(keyData);
      });
    }

    getLocalStorageInfo();

    function getZero(val) {
      return val < 10 ? `0${val}` : +val;
    }

    // delete save
    function deleteSave(idx) {
      const saveToDelete = saveNames[idx];
      console.log(saveToDelete);

      const gainedData = JSON.parse(localStorage.getItem("saves"));
      const newSaveData = gainedData.filter((el) => el.id !== saveToDelete.id);

      const parsed = JSON.stringify(newSaveData);
      localStorage.setItem("saves", parsed);
      saveNames.splice(idx, 1);
    }

    // GROUPS
    groupData = JSON.parse(localStorage.getItem("group"));

    function openGroupModal() {
      showModal.value = true;
      isGroupAdding.value = true;
    }

    function saveGroup() {
      if (characters && characters.length > 0) {
        const parsed = JSON.stringify(characters);
        localStorage.setItem("group", parsed);
      } else {
        localStorage.removeItem("group");
      }
      closeModal();
    }

    function loadGroup() {
      groupData.forEach((el) => {
        characters.push(el);
      });
      closeModal();
    }

    // settings
    function openSettingsModal() {
      showModal.value = true;
      isSettingsOpen.value = true;
    }

    function setFontSize(val) {
      document.body.style.fontSize = `${val}rem`;
      localStorage.setItem(`fontSize`, val);
    }
    // setFontSize(2.2);

    function clearSavesFromLocalStorage() {
      localStorage.removeItem("saves");
    }

    function clearGroupFromLocalStorage() {
      localStorage.removeItem("group");
    }

    // debugging
    const hideFromProduction = ref(false);
    // console.log(localStorage);

    function test() {
      let test = JSON.parse(localStorage.getItem("saves"));
      return test[0];
    }
    // console.log(test());

    return {
      characters,
      showModal,
      modalData,
      isMenuCoverShown,
      isMenuPositionBlocked,
      isStatAdding,
      isStatEditing,
      isNameEditing,
      isNodeEditing,
      isConditionAdding,
      isSaveLoading,
      isGroupAdding,
      hideFromProduction,
      isSettingsOpen,
      modalCalc,
      turnIdx,
      fullTurns,
      isSortingByReactionActive,
      saveNames,
      activeSave,
      newSaveName,
      groupData,
      addCharacter,
      addStat,
      plusStat,
      closeModal,
      removeCharacter,
      removeStat,
      chooseType,
      closeMenu,
      closeStatMenuByClickOnCover,
      openStatMenu,
      openCharMenu,
      changeStat,
      toggleStat,
      blockMenuPos,
      editStat,
      applyEdit,
      editName,
      applyName,
      makeTurn,
      resetTurns,
      sortById,
      sortByReaction,
      sortCharacters,
      moveStat,
      openConditionMenu,
      changeCondition,
      toggleCondition,
      removeCondition,
      plusCondition,
      addCondition,
      updateNote,
      spoilerNote,
      editNote,
      applyNodeEdit,
      openSaveModal,
      clearLocalStorage,
      addNewSaveToLocalStorage,
      rewriteSave,
      setMainSave,
      loadFromLocalStorage,
      deleteSave,
      openGroupModal,
      saveGroup,
      loadGroup,
      openSettingsModal,
      changeReaction,
      clearSavesFromLocalStorage,
      clearGroupFromLocalStorage,
      addNote,
      copyCharacter,
    };
  },
};
</script>

<style scoped lang="scss"></style>
