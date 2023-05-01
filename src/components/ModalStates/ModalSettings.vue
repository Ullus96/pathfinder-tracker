<template>
  <div class="modal__card">
    <div class="modal__header">
      <h2>Настройки</h2>
      <div class="modal__close" @click="closeModal">&#10005;</div>
    </div>
    <!-- modal main content -->
    <div class="modal__content modal__scrollable">
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
      <div class="modal__block">
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

      <!-- links -->
      <div class="modal__block">
        <div class="modal__title">
          <h4 class="modal__title--text">Ссылки</h4>
        </div>
        <p class="modal__author-text">
          <a href="https://github.com/Ullus96/pathfinder-tracker"
            ><i class="fa-brands fa-github"></i> Github</a
          >
        </p>
      </div>
      <!-- end of links -->

      <!-- btns -->
      <div class="modal__btns">
        <div class="btn btn-red" @click="closeModal">Закрыть</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["hideFromProduction"],
  emits: [
    "closeModal",
    "clearLocalStorage",
    "clearSavesFromLocalStorage",
    "clearGroupFromLocalStorage",
  ],
  setup(props, context) {
    function closeModal() {
      context.emit("closeModal");
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
      clearLocalStorage,
      clearSavesFromLocalStorage,
      clearGroupFromLocalStorage,
    };
  },
};
</script>

<style></style>
