<template>
  <input type="text" v-model="computedValue" />
</template>

<script>
export default {
  props: {
    value: String,
  },
  computed: {
    computedValue: {
      // 値を参照する。get
      get() {
        return this.value;
      },
      // 値を更新する。set
      set(value) {
        // 現在の値をemitで渡す。
        this.$emit("input", value);
      },
    },
  },
};
</script>