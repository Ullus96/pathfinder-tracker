<template>
  <!-- modal start -->
  <div class="modal" style="display: none">
    <div class="modal__card">
      <div class="modal__header">
        <h2>Добавить характеристику</h2>
        <div class="modal__close">&#10005;</div>
      </div>
      <!-- modal main content -->
      <div class="modal__content">
        <!-- types start -->
        <div class="modal__title">
          <h4 class="modal__title--text">Тип</h4>
        </div>
        <div class="modal__types">
          <div class="modal__type btn">Стат</div>
          <div class="modal__type btn active">Мин/макс</div>
          <div class="modal__type btn">Вкл/выкл</div>
          <div class="modal__type btn">ХП-бар</div>
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
            />
          </div>
          <div class="modal__value">
            <p>Знач.:</p>
            <input
              class="modal__input"
              type="text"
              placeholder="Цифровое значение"
            />
          </div>
          <div class="modal__value">
            <p>Макс:</p>
            <input
              class="modal__input"
              type="text"
              placeholder="Максимальное значение"
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
            <div class="modal__item btn">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn active">
              <i class="fa-solid fa-hand-fist"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-hat-wizard"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-dragon"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-dice-d6"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-dice-d20"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-shoe-prints"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-person-running"></i>
            </div>
            <div class="modal__item btn"><i class="fa-solid fa-eye"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-skull"></i></div>
            <div class="modal__item btn">
              <i class="fa-solid fa-shield-halved"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-rotate-left"></i>
            </div>
            <div class="modal__item btn"><i class="fa-solid fa-heart"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-brain"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-book"></i></div>
            <div class="modal__item btn"><i class="fa-solid fa-bolt"></i></div>
            <div class="modal__item btn">
              <i class="fa-solid fa-triangle-exclamation"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-diamond"></i>
            </div>
            <div class="modal__item btn">
              <i class="fa-solid fa-square"></i>
            </div>
            <div class="modal__item btn"><i class="fa-solid fa-fire"></i></div>
          </div>
        </div>
        <!-- end of icons -->

        <!-- color picker -->
        <div class="modal__block">
          <div class="modal__title">
            <h4 class="modal__title--text">Цвет</h4>
          </div>
          <div class="modal__flex">
            <div class="modal__item btn text-blue">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn text-red">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn text-yellow">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn active text-green">
              <i class="fa-solid fa-circle"></i>
            </div>
            <div class="modal__item btn text-white">
              <i class="fa-solid fa-circle"></i>
            </div>
          </div>
        </div>
        <!-- end of color -->

        <!-- btns -->
        <div class="modal__btns">
          <div class="btn btn-red">Отмена</div>
          <div class="btn">Применить</div>
        </div>
      </div>
    </div>
  </div>
  <!-- modal end -->
  <header class="header">
    <ul class="header__content">
      <li class="header__button" id="new-note">
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-regular fa-note-sticky"></i>
      </li>
      <li class="header__button" id="new-enemy">
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-solid fa-ghost"></i>
      </li>
      <li class="header__button" id="new-npc">
        <i class="fa-sharp fa-regular fa-plus"></i>
        <i class="fa-solid fa-user"></i>
      </li>
      <li class="header__button" id="toggle-menu">
        <i class="fa-regular fa-circle-question"></i>
      </li>
    </ul>
  </header>
  <section class="content">
    <!-- single person -->
    <div class="person">
      <div class="person__header">
        <div class="person__sort-btn">
          <i class="fa-solid fa-sort"></i>
        </div>
        <div class="person__title bg-blue">
          <div class="person__name">
            <i class="fa-solid fa-user"></i>
            Персонаж
          </div>
          <div class="person__edit"><i class="fa-solid fa-bars"></i></div>
        </div>
      </div>

      <div class="stats">
        <!-- armor -->
        <div class="stats__item bg-blue">
          <i class="fa-solid fa-shield-halved"></i>
          <p class="stats__item--value">17</p>
        </div>
        <!-- end of armor -->
        <!-- named with value -->
        <div class="stats__item bg-green">
          <i class="fa-solid fa-diamond"></i>
          <p class="stats__item--name">Focus Points</p>
          <div class="stats__item--value">
            <div class="value">
              <p class="value--current">1</p>
              <p class="value--breakpoint">/</p>
              <p class="value--total">3</p>
            </div>
          </div>
        </div>
        <!-- end of value -->
        <!-- toggleable -->
        <div class="stats__item bg-yellow">
          <i class="fa-solid fa-rotate-left"></i>
          <p class="stats__item--name">Reaction</p>
          <div class="stats__item--value">
            <div class="toggle">
              <div class="toggle-on"><i class="fa-solid fa-toggle-on"></i></div>
              <!-- <div class="toggle-off">
                <i class="fa-solid fa-toggle-off"></i>
              </div> -->
            </div>
          </div>
        </div>
        <!-- end of toggleable -->
        <!-- plus -->
        <div class="stats__item stats__item--add">
          <i class="fa-solid fa-plus"></i>
        </div>
        <!-- end of plus -->
      </div>
    </div>
    <!-- end of single person -->

    <!-- single person -->
    <div class="person">
      <div class="person__header">
        <div class="person__sort-btn">
          <i class="fa-solid fa-sort"></i>
        </div>
        <div class="person__title bg-red">
          <div class="person__name">
            <i class="fa-solid fa-ghost"></i>
            Лич Бальтазар
          </div>
          <div class="person__edit"><i class="fa-solid fa-bars"></i></div>
        </div>
      </div>

      <div class="stats">
        <!-- hp -->
        <div class="stats__item stats__item--wide bg-red">
          <i class="fa-solid fa-heart"></i>
          <div class="stats__item--value">
            <div class="value">
              <p class="value--current">27</p>
              <p class="value--breakpoint">/</p>
              <p class="value--total">50</p>
            </div>
          </div>
          <div class="health">
            &nbsp;
            <div class="health--current-bar"></div>
            <p class="health--amount">54%</p>
          </div>
        </div>
        <!-- end of hp -->
        <!-- desc -->
        <div class="stats__item bg-white">
          <i class="fa-solid fa-book"></i>
          <p class="stats__item--name">Нежить</p>
        </div>
        <!-- end of desc -->
        <!-- armor -->
        <div class="stats__item bg-blue">
          <i class="fa-solid fa-shield-halved"></i>
          <p class="stats__item--value">17</p>
        </div>
        <!-- end of armor -->
        <!-- named with value -->
        <div class="stats__item bg-green">
          <i class="fa-solid fa-diamond"></i>
          <p class="stats__item--name">Focus Points</p>
          <div class="stats__item--value">
            <div class="value">
              <p class="value--current">1</p>
              <p class="value--breakpoint">/</p>
              <p class="value--total">3</p>
            </div>
          </div>
        </div>
        <!-- end of value -->
        <!-- toggleable -->
        <div class="stats__item bg-yellow">
          <i class="fa-solid fa-rotate-left"></i>
          <p class="stats__item--name">Reaction</p>
          <div class="stats__item--value">
            <div class="toggle">
              <div class="toggle-on"><i class="fa-solid fa-toggle-on"></i></div>
              <!-- <div class="toggle-off">
                <i class="fa-solid fa-toggle-off"></i>
              </div> -->
            </div>
          </div>
        </div>
        <!-- end of toggleable -->
        <!-- plus -->
        <div class="stats__item stats__item--add">
          <i class="fa-solid fa-plus"></i>
        </div>
        <!-- end of plus -->
      </div>
    </div>
    <!-- end of single person -->
  </section>
</template>

<script>
export default {};
</script>

<style scoped lang="scss"></style>
