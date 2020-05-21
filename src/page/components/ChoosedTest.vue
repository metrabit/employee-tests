<template>
  <div class="choosed-test">
    <h4 class="choosed-test__title">{{ data.codigo }}</h4>
    <p class="choosed-test__description">{{ data.fantasia }}</p>
    <div class="choosed-test__input-container">
      <Input
        placeholder="Ej: 20"
        type="number"
        :value="changedData.value"
        class="choosed-test__input"
        :error="isErrorMessage"
        @input-value="test"
      />
    </div>
  </div>
</template>

<script>
import Input from "./Input";
export default {
  name: "ChoosedTest",
  components: {
    Input
  },
  props: {
    data: {
      required: true,
      type: Object
    },
    isError: Boolean
  },
  data: () => ({
    value: null
  }),
  computed: {
    changedData() {
      return { ...this.data, points: this.value };
    },
    isErrorMessage() {
      return this.isError ? " " : "";
    }
  },
  methods: {
    test(e) {
      this.value = e.target.value;
      this.$emit("get-value", this.changedData);
    }
  }
};
</script>

<style lang="scss">
@import "../style/style";

.choosed-test {
  display: flex;
  justify-content: space-between;
  align-items: center;

  @media (max-width: 992px) {
    display: block;
  }

  &__title {
    color: $title-color;
    font-size: 18px;
    margin: 0;
    margin-right: 16px;

    @media (max-width: 992px) {
      display: inline-block;
      margin-bottom: 16px;
    }
  }

  &__description {
    margin: 0;
    color: $text-color;

    @media (max-width: 992px) {
      display: inline-block;
      margin-bottom: 16px;
    }
  }

  &__input-container {
    flex: 1;
    display: flex;
    justify-content: flex-end;
    height: fit-content;
  }

  &__input {
    width: 225px;

    @media (max-width: 992px) {
      width: 100%;
    }
  }
}
</style>