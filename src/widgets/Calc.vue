<template>
  <section class="calc">
    <form @submit.prevent="onCalcSubmit" class="calc-form">
      <div class="calc-wall">
        <div class="field">
          <label for="wallWidth">Ширина стены</label>
          <input type="number" step=".1" id="wallWidth" v-model="wallWidth" />
          <span>м.</span>
        </div>
        <div class="field">
          <label for="wallHeight">Высота стены</label>
          <input type="number" step=".1" id="wallHeight" v-model="wallHeight" />
          <span>м.</span>
        </div>
      </div>
      <div class="calc-opening" v-for="(opening, idx) in openings" :key="idx">
        <div class="field field--opening">
          <label for="wallWidth">Ширина</label>
          <input type="number" step=".1" id="wallWidth" v-model="opening.width" />
          <span>см.</span>
        </div>
        <div class="field field--opening">
          <label for="wallHeight">Высота</label>
          <input type="number" step=".1" id="wallHeight" v-model="opening.height" />
          <span>см.</span>
        </div>
      </div>
      <ui-button class="btn" type="button" theme="hollow" @click="onAddOpening"
        >Добавить проём
      </ui-button>
      <ui-button class="btn" type="submit">Считать</ui-button>
    </form>
    <div class="result">
      <ui-alert success v-if="isResultShown">
        На данную перегородку потребуется <strong>{{ boards.amount }} шт.</strong> Пазогребневых
        Гипсовых Плит общим весом <strong>{{ boards.weight }} кг</strong></ui-alert
      >
      <ui-alert error v-if="isError"> {{ errorMsg }}</ui-alert>
    </div>
    <div></div>
  </section>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import UiAlert from '../shared/Alert/UiAlert.vue';
import UiButton from '../shared/Button/UiButton.vue';

const isResultShown = ref(false);
const isError = ref(false);
const errorMsg = ref('');

const boardParams = {
  width: 0.5,
  height: 0.5,
  weight: 36,
};
const boardSquare = boardParams.width * boardParams.height;

const wallWidth = ref(4.2);
const wallHeight = ref(2.7);

interface OpeningParams {
  width: number;
  height: number;
}
const openings = ref<OpeningParams[]>([]);

const boards = ref({ amount: 0, weight: 0 });

const onAddOpening = () => {
  const defaultOpeningParams = { width: 90, height: 205 };
  openings.value = [...openings.value, defaultOpeningParams];
};

const onCalcSubmit = () => {
  const wallSquare = wallWidth.value * wallHeight.value;
  const openingsSquare = openings.value.reduce(
    (square, opening) => square + (opening.width / 100) * (opening.height / 100),
    0,
  );
  const boardsAmount = (wallSquare - openingsSquare) / boardSquare;
  if (boardsAmount < 0) {
    isResultShown.value = false;
    isError.value = true;
    errorMsg.value =
      'Кажется, вы допустили ошибку в параметрах. Пожалуйста, проверьте и обновите значения';
  } else {
    boards.value.amount = Math.ceil(boardsAmount);
    // Предполагаю, что плиты могут обрезаться и вес стены нужно считать
    // от неокругленного количества плит
    boards.value.weight = Number((boardsAmount * boardParams.weight).toFixed(2));
    isError.value = false;
    errorMsg.value = '';
    isResultShown.value = true;
  }
};
</script>

<style scoped lang="scss">
.calc {
  max-width: 600px;
  margin: 0 auto;
  .field {
    margin: 1rem 0;
    label {
      margin-right: 0.75em;
    }
    span {
      margin-left: 0.5em;
    }
    input {
      display: inline-block;
      width: 45px;
    }
    &--opening {
      margin: unset;
    }
  }

  &-opening {
    display: flex;
    gap: 0.75rem;
    border-top: 1px solid #d5d8de;
    border-bottom: 1px solid #d5d8de;
    margin: 1rem 0;
    padding: 0.5rem 0;
  }

  .btn {
    margin-right: 1rem;
  }
}
</style>
