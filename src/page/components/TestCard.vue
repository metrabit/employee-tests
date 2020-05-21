<template>
  <div class="test-card" :class="{'test-card--checked': isChecked}">
    <div class="test-card__container">
      <div class="test-card__container test-card__title-container">
        <h4 class="test-card__title">{{ data.codigo }}</h4>
        <InfoIcon
          @mouseenter.native="showNotice($event)"
          @mouseleave.native="hideNotice($event)"
          class="test-card__notice-icon"
        />
        <div class="test-card__notice-box">
          <p
            class="test-card__notice-text"
          >Comprensión verbal, concepción espacial, razonamiento, cálculo numérico y fluidez verbal. Predictor de inteligencia global (IG).</p>
        </div>
      </div>
      <input type="checkbox" class="test-card__checkbox" v-model="isChecked" @input="checkCard" />
    </div>
    <p class="test-card__description">{{ data.fantasia }}</p>
    <div class="test-card__container">
      <a :href="data.informe_tipo" target="_blank" class="test-card__link">Ver ficha tecnica</a>
      <div class="test-card__container">
        <ClockIcon />
        <p class="test-card__time">{{ data.tiempo }} min</p>
      </div>
    </div>
  </div>
</template>

<script>
import ClockIcon from "../icons/ClockIcon";
import InfoIcon from "../icons/InfoIcon";

export default {
  name: "TestCard",
  components: {
    ClockIcon,
    InfoIcon
  },
  props: {
    data: {
      required: true,
      type: Object
    }
  },
  computed: {
    changedData() {
      return { ...this.data, isChecked: this.isChecked };
    }
  },
  data: () => ({
    isChecked: false
  }),
  methods: {
    showNotice(e) {
      e.target
        .closest(".test-card__container")
        .querySelector(".test-card__notice-box")
        .classList.add("showed");
    },
    hideNotice(e) {
      e.target
        .closest(".test-card__container")
        .querySelector(".test-card__notice-box")
        .classList.remove("showed");
    },
    checkCard() {
      this.isChecked = !this.isChecked;
      this.$emit("check", this.changedData);
    }
  }
};
</script>

<style lang="scss">
@import "../style/style.scss";

.test-card {
  width: 254px;
  border: 1px solid $secondary-grey;
  box-shadow: 0px 1px 1px 0px $secondary-grey;
  padding: 16px;
  box-sizing: border-box;
  margin-right: 12px;
  margin-bottom: 12px;

  &--checked {
    border: 2px solid $green;
    box-shadow: none;
  }

  &__container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: fit-content;
  }

  &__title {
    margin: 0;
    margin-right: 4px;
    color: $title-color;
  }

  &__title-container {
    position: relative;
  }

  &__notice-box {
    position: absolute;
    bottom: 32px;
    left: -110px;
    width: 260px;
    transition: opacity 0.3s ease-out;
    visibility: hidden;
    padding: 8px 21px;
    background-color: rgba(92, 92, 91, 0.9);
  }

  &__notice-text {
    margin: 0;
    font-size: 13px;
    line-height: 18px;
    color: #fff;

    &::after {
      content: " ";
      position: absolute;
      right: 138px;
      bottom: -8px;
      border-top: 8px solid rgba(92, 92, 91, 0.9);
      border-right: 8px solid transparent;
      border-left: 8px solid transparent;
      border-bottom: none;
    }
  }

  &__description {
    margin-top: 8px;
    margin-bottom: 4px;
    font-size: 14px;
    color: $text-color;
  }

  &__link {
    font-size: 15px;
    color: #0274be;

    &:hover {
      color: #4a90e2;
    }

    &:visited {
      color: #0274be;
    }
  }

  &__time {
    margin: 0;
    margin-left: 4px;
    font-size: 12px;
    color: $text-color;
  }

  &__checkbox {
    height: 18px;
    width: 19px;
    position: relative;
    cursor: pointer;

    &:after {
      content: "\00D7";
      content: "\00D7";
      display: block;
      pointer-events: none;
      font-size: 12px;
      background: #fff;
      position: absolute;
      top: 0;
      left: 0;
      height: 15px;
      width: 15px;
      color: transparent;
      transition: 0.25s all ease-in-out;
      border-radius: 3px;
      line-height: 26px;
      background-image: url("../../assets/interface.svg");
      background-position: 0 15px;
      background-repeat: no-repeat;
      background-size: 15px 12px;
      border: 1px solid $light-grey;
      z-index: 99;
      background-position: 0 0;
    }

    &:checked:after {
      border: 1px solid $green;
      background: $green;
      background-image: url("../../assets/interface.svg");
      background-position: 3px 3px;
      background-repeat: no-repeat;
      background-size: 10px 10px;
    }
  }
}
</style>