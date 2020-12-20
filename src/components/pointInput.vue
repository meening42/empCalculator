<template>
  <div>
    <div>
      <span></span>
      <span class="cordinate">x</span>
      <input
        name="x"
        type="number"
        ref="x"
        :value="T.x"
        v-on:input="onValueChange($event)"
      /><span>m</span>
      <span class="cordinate">y</span>
      <input
        name="y"
        type="number"
        ref="y"
        :value="T.y"
        v-on:input="onValueChange($event)"
      /><span>m</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "pointInput",
  props: {
    T: Object,
    pCharges: Object,
  },
  methods: {
    onValueChange(event) {
      this.$emit("point-changed", event.target.name, event.target.value);
    },
  },
};
</script>

<style scoped>
input {
  width: 60px;
}

.cordinate {
  margin-left: 10px;
}
</style>
