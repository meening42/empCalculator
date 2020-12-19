<template>
  <div class="inputs">
    <span>q{{index}}</span>
    <input name ="q" type="number" v-bind:value="propCharge.q" v-on:input="onValueChange($event)"/>
    <span>As/m</span>
    <div v-if="propCharge.x">
      <span class="cordinate">x_</span>
      <input name ="x" type="number" :value="propCharge.x" v-on:input="onValueChange($event)">
      <span>m</span>
    </div>
    <div  v-if="propCharge.x_z">
      <span class="cordinate">xz</span>
      <input name ="x_z" type="number" :value="propCharge.x_z" v-on:input="onValueChange($event)">
      <span>m</span>
    </div>
    <div  v-if="propCharge.x_k">
      <span class="cordinate">xk</span>
      <input name ="x_k" type="number" :value="propCharge.x_k" v-on:input="onValueChange($event)">
      <span>m</span>
    </div>
    <div  v-if="propCharge.y">
      <span class="cordinate">y_</span>
      <input name ="y" type="number" :value="propCharge.y" v-on:input="onValueChange($event)">
      <span>m</span>
    </div>
    <div  v-if="propCharge.y_z">
      <span class="cordinate">yz</span>
      <input name ="y_z" type="number" :value="propCharge.y_z" v-on:input="onValueChange($event)">
      <span>m</span>
    </div>
    <div  v-if="propCharge.y_k">
      <span class="cordinate">yk</span>
      <input name ="y_k" type="number" :value="propCharge.y_k" v-on:input="onValueChange($event)">
      <span>m</span>
    </div>
  </div>
</template>

<script>
export default{
  name: "finLineCharge",
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
  .inputs{
    display: flex;
    justify-content: center;
  }
</style>
