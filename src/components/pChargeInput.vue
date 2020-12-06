<template>
  <div>
    <div>
      <span>Q{{index}}</span>
      <input name ="Q" type="number" v-bind:value="propCharge.Q" v-on:input="onValueChange($event)"/><span>As</span>
      <span class="cordinate">x</span>
      <input name ="x" type="number" :value="propCharge.x" v-on:input="onValueChange($event)"><span>m</span>
      <span class="cordinate">y</span>
      <input name ="y" type="number" :value="propCharge.y" v-on:input="onValueChange($event)"><span>m</span>
    </div>
  </div>
</template>

<script>
export default{
  name: "pChargeInput",
  props: {
    propCharge: Object,
    index: Number,
  },
  methods:{
    onValueChange(event){
      this.$emit("charge-changed",event.target.name,event.target.value, this.$props.index);
    }
  }
}
</script>

<style scoped>
  input{
    width: 60px;
  }

  .cordinate{
    margin-left: 10px;
  }
</style>
