<template>
  <div class="tests-input">
    <label v-if="label" class="tests-input__label">{{ label }}</label>
    <input
      :type="type"
      :placeholder="placeholder"
      :maxlength="maxLength"
      min="0"
      class="tests-input__input"
      :class="{'tests-input__input--error': error}"
      @input="setValue($event)"
    />
    <p v-if="isShowUndertext" class="tests-input__undertext">{{ undertext }}</p>
    <p v-if="error" class="tests-input__error">{{ error }}</p>
  </div>
</template>

<script>
export default {
  name: "Input",
  props: {
    label: String,
    placeholder: String,
    undertext: String,
    maxLength: Number,
    type: {
      default: "text",
      type: String
    },
    error: String
  },
  computed: {
    isShowUndertext() {
      return !this.error && this.undertext;
    },
    changedValue() {
      return this.value;
    }
  },
  methods: {
    setValue($event) {
      this.$emit("input-value", $event);
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../style/style.scss";

.tests-input {
  display: flex;
  flex-direction: column;

  &__label {
    color: $title-color;
    margin-bottom: 8px;
    font-weight: 700;
    font-size: 16px;
  }

  &__input {
    padding: 12px;
    border: 1px solid $light-grey;

    &::placeholder {
      color: $secondary-grey;
    }

    &[type="number"]::-webkit-inner-spin-button,
    &[type="number"]::-webkit-outer-spin-button {
      appearance: none;
      margin: 0;
    }

    &--error {
      border: 1px solid $red;
    }
  }

  &__undertext {
    margin-top: 6px;
    font-size: 14px;
    color: $secondary-grey;
  }

  &__error {
    margin-top: 6px;
    font-size: 14px;
    color: $red;
  }
}
</style>