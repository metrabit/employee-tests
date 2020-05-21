<template>
  <div class="tests">
    <div class="tests-data">
      <h2 class="tests-data__title">1. Datos KeyScoring</h2>
      <p
        class="tests-data__description"
      >Ingresa el nombre y código con el que identificarás este KeyScoring.</p>
      <div class="tests-data__inputs">
        <Input
          label="Nombre"
          placeholder="Ej: Ejecutivo comercial"
          :error="occupationInputValueError"
          class="tests-data__input"
          @input-value="setValue($event, 'occupationInputValue')"
        />
        <Input
          label="Código"
          :maxLength="5"
          placeholder="Ej: ECOM"
          undertext="De 3 a 5 caracteres"
          :error="codeInputValueError"
          class="tests-data__input"
          @input-value="setValue($event, 'codeInputValue')"
        />
      </div>
    </div>
    <div class="tests-proofs">
      <h2 class="tests-proofs__title">2. Selección de pruebas</h2>
      <div class="tests-proofs__advices">
        <div class="tests-proofs__advice">
          <p
            class="tests-proofs__advice-text"
          >Selecciona la(s) prueba(s) que utilizarás en este KeyScoring. Podrás seleccionar más de una en cada categoría.</p>
        </div>
        <div class="tests-proofs__advice tests-proofs__advice--colorful">
          <LampIcon class="tests-proofs__advice-icon" />
          <p
            class="tests-proofs__advice-text tests-proofs__advice-text--colorful"
          >Te recomendamos no aplicar más de 3 pruebas por KeyScoring y al menos una por cada categoría.</p>
        </div>
      </div>
      <div v-show="hasntCheckedTests" class="tests-own">
        <p
          class="tests-own__text tests-own__text--red js-empty-tests-error"
        >Debes seleccionar alguna prueba</p>
      </div>
      <div v-if="professionalSkills.length" class="tests-proofs__skills">
        <h3 class="tests-proofs__skills-title">Habilidades y Destrezas</h3>
        <div class="tests-proofs__skills-cards">
          <TestCard
            v-for="card in professionalSkills"
            :key="card.id"
            :data="card"
            @check="getCheckedCards($event)"
          />
        </div>
      </div>
      <div v-if="intelligenceSkills.length" class="tests-proofs__skills">
        <h3 class="tests-proofs__skills-title">Capacidades Intelectuales</h3>
        <div class="tests-proofs__skills-cards">
          <TestCard
            v-for="card in intelligenceSkills"
            :key="card.id"
            :data="card"
            @check="getCheckedCards($event)"
          />
        </div>
      </div>
      <div v-if="personalSkills.length" class="tests-proofs__skills">
        <h3 class="tests-proofs__skills-title">Personalidad y Comportamiento</h3>
        <div class="tests-proofs__skills-cards">
          <TestCard
            v-for="card in personalSkills"
            :key="card.id"
            :data="card"
            @check="getCheckedCards($event)"
          />
        </div>
      </div>
      <div v-if="personalSkills.length" class="tests-proofs__skills">
        <h3 class="tests-proofs__skills-title">Pruebas Propias</h3>
        <div class="tests-proofs__skills-cards">
          <TestCard
            v-for="card in ownTests"
            :key="card.id"
            :data="card"
            @check="getCheckedCards($event)"
          />
        </div>
      </div>
      <div v-if="isShowHelper" class="tests-own">
        <p
          class="tests-own__text tests-own__text--yellow"
        >Al seleccionar 5 o más pruebas, el tiempo de rendición podría afectar los resultados de la evaluación</p>
      </div>
      <div v-show="checkedTests.length" class="tests-totals">
        <h3 class="tests-totals__title">Ponderación de pruebas</h3>
        <p
          class="tests-totals__text"
        >Asigna el nivel de importancia de cada una de las pruebas expresado en porcentaje. La suma debe ser 100%.</p>
        <div class="tests-totals__container-main">
          <div class="tests-totals__container">
            <div class="tests-totals__counters">
              <div class="tests-totals__counters-test tests-totals__counters-test--bordered">
                Pruebas seleccionadas
                <span
                  class="tests-totals__counter--bold"
                >{{ checkedTests.length }}</span>
              </div>
              <div class="tests-totals__counters-test">
                <ClockIcon :height="'24px'" class="tests-totals__time-icon" />Tiempo rendición pruebas
                <span
                  class="tests-totals__counter--bold"
                >{{ totalTimeForTests }} min</span>
              </div>
            </div>
            <div class="tests-totals__choosed-tests">
              <div class="tests-totals__table-headers">
                <h3 class="tests-totals__table-title">Pruebas</h3>
                <h3 class="tests-totals__table-title">Ponderación (%)</h3>
              </div>
              <ChoosedTest
                v-for="item in checkedTests"
                :key="item.id"
                :data="item"
                :is-error="isChoosedTestError"
                @get-value="getValueFromChoosedTests($event)"
                class="tests-totals__choosed-test"
              />

              <div v-show="isChoosedTestError" class="tests-own">
                <p
                  class="tests-own__text tests-own__text--red js-empty-tests-error"
                >Debes asignar ponderación a todas las pruebas</p>
              </div>
              <div v-show="isNotCorrectTotalResults && !isChoosedTestError" class="tests-own">
                <p
                  class="tests-own__text tests-own__text--red js-empty-tests-error"
                >La suma de las ponderaciones debe sumar 100</p>
              </div>
            </div>
          </div>
          <div class="tests-totals__container-total">
            <div class="tests-totals__total-weight">
              <h3 class="tests-totals__total-weight-title">Total ponderación</h3>
              <div
                class="tests-totals__total-weight-result"
                :class="{
                'tests-totals__total-weight-result--red': isRed,
                'tests-totals__total-weight-result--yellow': isYellow,
                'tests-totals__total-weight-result--green': isGreen,
              }"
              >{{ totalPointsForTests }}%</div>
              <p class="tests-totals__total-weight-description">{{ totalPointsMessage }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="tests-buttons">
      <div class="tests-buttons__return-btn">Volver</div>
      <div class="tests-buttons__clear-btn" @click="validateInputs">Crear KeyScoring</div>
    </div>
  </div>
</template>
  
<script>
// import axios from "axios";

import data from "./data/data";

import Vue from "vue";

import Input from "./components/Input";
import TestCard from "./components/TestCard";
import ChoosedTest from "./components/ChoosedTest";
import LampIcon from "./icons/LampIcon";
import ClockIcon from "./icons/ClockIcon";

export default {
  name: "Page",
  components: {
    Input,
    ChoosedTest,
    TestCard,
    LampIcon,
    ClockIcon
  },
  data: () => ({
    tests: {},
    personalSkills: [],
    intelligenceSkills: [],
    professionalSkills: [],
    ownTests: [],
    checkedTests: [],
    pointsFromChoosedTests: [],
    occupationInputValue: "",
    isOccupationInputValueError: false,
    codeInputValue: "",
    isCodeInputValueError: false,
    isChoosedTestError: false,
    hasntCheckedTests: false,
    isNotCorrectTotalResults: false
  }),
  computed: {
    isShowHelper() {
      return this.checkedTests.length >= 5;
    },
    totalTimeForTests() {
      return this.checkedTests.reduce((acc, item) => acc + item.tiempo, 0);
    },
    totalPointsForTests() {
      return this.pointsFromChoosedTests.reduce(
        (acc, item) => acc + Number(item.points),
        0
      );
    },
    isRed() {
      return this.totalPointsForTests > 100;
    },
    isYellow() {
      return this.totalPointsForTests < 100 && this.totalPointsForTests !== 0;
    },
    isGreen() {
      return this.totalPointsForTests === 100;
    },
    occupationInputValueError() {
      return this.isOccupationInputValueError
        ? "Debes ingresar un nombre para el cargo"
        : "";
    },
    codeInputValueError() {
      return this.isCodeInputValueError
        ? "Debes ingresar un mínimo de 3 caracteres y máximo de 5 caracteres"
        : "";
    },
    totalPointsMessage() {
      if (this.totalPointsForTests > 0 && this.totalPointsForTests < 100) {
        return `Falta ${100 - this.totalPointsForTests}% por asignar`;
      }

      if (this.totalPointsForTests === 100) {
        return `Asignación completa`;
      }

      if (this.totalPointsForTests > 100) {
        return "Ponderación sobrepasa el 100%";
      }

      return "No has asignado ningún valor";
    }
  },
  created() {
    // call method for correct data
    // this.getData();
    this.temporaryGetData();
  },
  methods: {
    // This method for your correct data, you should uncomment axios library on first row in tag script
    // getData() {
    //   axios
    // .get("")
    // .then(res => this.tests = res.data.tests)
    // .catch(error => console.log('something wrong'))
    // }
    temporaryGetData() {
      const personalSkills = "Personalidad y Comportamiento";
      const intelligenceSkills = "Capacidades Intelectuales";
      const professionalSkills = "Habilidades y Destrezas";
      const ownTests = "Pruebas Propias";

      this.tests = data;
      this.personalSkills = data.tests[personalSkills];
      this.intelligenceSkills = data.tests[intelligenceSkills];
      this.professionalSkills = data.tests[professionalSkills];
      this.ownTests = data.tests[ownTests];
    },
    getCheckedCards(e) {
      if (e.isChecked === true) {
        this.checkedTests.push(e);
      } else {
        this.checkedTests = this.checkedTests.filter(test => test.id !== e.id);

        this.pointsFromChoosedTests.forEach((item, index) => {
          if (item.id === e.id) {
            this.pointsFromChoosedTests.splice(index, 1);
          }
        });
      }
    },
    getValueFromChoosedTests(e) {
      if (!this.pointsFromChoosedTests.some(item => item.id === e.id)) {
        this.pointsFromChoosedTests.push(e);
      } else {
        this.pointsFromChoosedTests.forEach((item, index) => {
          if (item.id === e.id) {
            Vue.set(this.pointsFromChoosedTests, index, e);
          }
        });
      }
    },
    validateInputs() {
      this.occupationInputValue === ""
        ? (this.isOccupationInputValueError = true)
        : (this.isOccupationInputValueError = false);

      this.codeInputValue === "" ||
      this.codeInputValue.length < 3 ||
      this.codeInputValue.length > 5
        ? (this.isCodeInputValueError = true)
        : (this.isCodeInputValueError = false);

      this.checkedTests.length === 0
        ? (this.hasntCheckedTests = true)
        : (this.hasntCheckedTests = false);

      this.totalPointsForTests === 100
        ? (this.isNotCorrectTotalResults = false)
        : (this.isNotCorrectTotalResults = true);

      const counterOfFilledInputs = this.pointsFromChoosedTests.filter(
        item => item.points !== ""
      );

      this.isChoosedTestError =
        counterOfFilledInputs.length !== this.checkedTests.length;

      if (this.isCodeInputValueError || this.isOccupationInputValueError) {
        window.scrollTo(top);
      } else if (this.hasntCheckedTests) {
        const helperBlock = document.querySelector(".js-empty-tests-error");

        if (helperBlock) {
          helperBlock.scrollIntoView({ block: "center", behavior: "smooth" });
        }
      }
    },
    setValue($event, variable) {
      Vue.set(this, variable, $event.target.value);
    }
  }
};
</script>

<style lang="scss" scoped>
@import "./style/style";

.tests {
  display: flex;
  flex-direction: column;
  margin-left: 13.774%;
  margin-right: 13.774%;

  @media (max-width: 992px) {
    max-width: 750px;
  }
}

// 1. Datos KeyScoring

.tests-data {
  display: flex;
  flex-direction: column;
  width: 100%;
  border-bottom: 1px solid $light-grey;

  &__title {
    color: $title-color;

    @media (max-width: 992px) {
      margin-bottom: 0;
    }
  }

  &__description {
    color: $text-color;
    margin-bottom: 64px;
  }

  &__inputs {
    display: flex;
    justify-content: space-between;

    @media (max-width: 992px) {
      flex-direction: column;
    }
  }

  &__input {
    flex: 1;
    margin-bottom: 32px;

    &:first-child {
      padding-right: 64px;

      @media (max-width: 992px) {
        padding-right: 0;
      }
    }
  }
}

//2. Selección de pruebas

.tests-proofs {
  &__title {
    color: $title-color;
  }

  &__advices {
    display: flex;
    margin-bottom: 64px;

    @media (max-width: 992px) {
      flex-direction: column;
      margin-bottom: 32px;
    }
  }

  &__advice {
    display: flex;
    flex: 1;
    height: fit-content;
    padding: 16px 0;

    &:first-child {
      padding-right: 64px;

      @media (max-width: 992px) {
        padding-right: 0;
      }
    }

    &--colorful {
      padding: 16px 16px 16px 0;
      background-color: $advice;
      max-width: calc(50% - 48px);

      @media (max-width: 992px) {
        max-width: 100%;
      }
    }
  }

  &__advice-icon {
    margin-left: 16px;
  }

  &__advice-text {
    margin: 0;
    color: $text-color;

    &--colorful {
      flex: 1;
      margin-left: 8px;
    }
  }

  &__skills-title {
    font-size: 18px;
    color: $title-color;
  }

  &__skills-cards {
    display: flex;
    flex-wrap: wrap;
  }
}

// helper if you chooce more than 4 tests

.tests-own {
  margin-bottom: 50px;

  &__text {
    padding: 24px 33px 20px;
    margin: 0;
    color: $text-color;
    border: 1px solid;
    border-left-width: 6px;

    &--yellow {
      border-color: $yellow;
    }

    &--red {
      border-color: $red;
    }
  }
}

//total weight of choosed tests & results

.tests-totals {
  &__container {
    &-main {
      display: flex;
      margin-bottom: 80px;
      padding-bottom: 80px;
      border-bottom: 1px solid $light-grey;

      @media (max-width: 1280px) {
        flex-direction: column;
        margin-bottom: 20px;
        padding-bottom: 20px;
      }
    }

    &-total {
      flex: 1;
      display: flex;
      justify-content: center;
      padding-top: 134px;

      @media (max-width: 1280px) {
        justify-content: flex-end;
        padding-top: 0px;
      }
    }
  }

  &__title {
    margin-bottom: 16px;
    font-size: 18px;
    color: $title-color;
  }

  &__text {
    color: $text-color;
  }

  &__counters {
    display: flex;
    width: fit-content;
    padding: 12px 80px 12px 32px;
    margin-bottom: 32px;
    border: 1px solid $light-grey;
    border-top: 2px solid $green;

    @media (max-width: 992px) {
      flex-direction: column;
      align-items: center;
      padding: 8px 32px;
    }
  }

  &__counters-test {
    display: flex;
    align-items: center;

    &:first-child {
      @media (max-width: 992px) {
        margin-bottom: 8px;
      }
    }

    &--bordered {
      border-right: 1px solid $light-grey;
      padding-right: 24px;
      margin-right: 24px;

      @media (max-width: 992px) {
        padding-right: 0;
        margin-right: 0;
        border: none;
      }
    }
  }

  &__counter {
    &--bold {
      margin-left: 24px;
    }
  }

  &__time-icon {
    margin-right: 8px;
  }

  &__table-headers {
    display: flex;
    justify-content: space-between;

    @media (max-width: 992px) {
      flex-direction: column;
    }
  }

  &__table-title {
    color: $title-color;
    font-size: 16px;

    &:last-child {
      margin-right: 100px;
    }

    @media (max-width: 992px) {
      margin-top: 0;
    }
  }

  &__choosed-test {
    margin-bottom: 32px;
  }

  &__total-weight {
    height: fit-content;
    border: 1px solid $light-grey;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: $title-color;

    &-title {
      text-align: center;
      margin-top: 0;
      padding: 4px 0;
      width: 100%;
      border-bottom: 1px solid $light-grey;
      font-size: 14px;
    }

    &-result {
      font-size: 50px;
      border-bottom: 1px solid $light-grey;
      width: 100%;
      text-align: center;
      padding-bottom: 10px;

      &--red {
        color: #b71c1c;
      }

      &--yellow {
        color: $yellow;
      }

      &--green {
        color: $green;
      }
    }

    &-description {
      font-size: 12px;
      padding: 0 8px;
    }
  }
}

.tests-buttons {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 150px;
  margin-top: 80px;

  @media (max-width: 1280px) {
    margin-top: 20px;
    margin-bottom: 40px;
  }

  &__return-btn {
    margin-right: 24px;
    padding: 12px 40px;
    transition: 0.3s;
    cursor: pointer;

    &:hover {
      border: 1px solid #0274be;
      box-shadow: 1px 3px 6px 0px rgba(0, 0, 0, 0.5);
    }
  }

  &__clear-btn {
    padding: 12px 40px;
    border-radius: 3px;
    transition: 0.3s;
    background-color: #0274be;
    color: #fff;
    cursor: pointer;

    &:hover {
      background-color: #4a90e2;
      box-shadow: 1px 3px 6px 0px rgba(0, 0, 0, 0.5);
    }
  }
}
</style>