<template>
  <div>
    <ExerciseHeader :exTitle="exTitle" :exText="exText"></ExerciseHeader>
    <pChargeInput
      v-for="(propCharge, index) in pCharges"
      :key="index"
      v-bind:propCharge="propCharge"
      v-bind:index="index"
      v-on:charge-changed="onChargeChange"
    >
    </pChargeInput>
    <pointInput
      v-bind:T="T"
      v-bind:pCharges="pCharges"
      v-on:point-changed="onPointChange"
    >
    </pointInput>
    <keep-alive>
    <graph2d :pCharges="pCharges" v-bind:T="T"></graph2d>
    </keep-alive>
    <button v-on:click="calculateEx1">Izračunaj</button>
    <p>Ex = {{ Ex }} Ey = {{ Ey }}</p>
  </div>
</template>

<script>
import pChargeInput from "../components/pChargeInput";
import pointInput from "../components/pointInput";
import ExerciseHeader from "../components/ExerciseHeader";
import graph2d from "../components/graph2d.vue";

export default {
  name: "ex2",
  components: {
    pChargeInput,
    pointInput,
    ExerciseHeader,
    graph2d,
  },
  data: function() {
    return {
      pCharges: [
        {
          Q: -1.2e-8,
          x: 5,
          y: 2,
        },
        {
          Q: 1.4e-8,
          x: 22,
          y: 8,
        },
        {
          Q: -1.5e-8,
          x: 10,
          y: 19,
        },
      ],
      V: 0,
      Ex: 0,
      Ey: 0,
      T: {
        x: 2,
        y: 11,
      },
      exTitle: "2. naloga",
      exText:
        "Določite E. Izvori so tri naelektrene premice (ležijo v pravokotno na xy ravnino):",
    };
  },
  methods: {
    onChargeChange(name, value, index) {
      this.$data.pCharges[index][name] = value;
      this.$data.pCharges = [...this.$data.pCharges];
    },
    onPointChange(name, value) {
      this.$data.T[name] = value;
      this.$data.T = { ...this.$data.T };
    },
    calculateEx1: function() {
      let i, r, Tx, Ty, Qx, Qy, Qi, Etemp, Ex, Ey;
      Tx = this.$data.T.x;
      Ty = this.$data.T.y;
      Ex = 0;
      Ey = 0;
      for (i = 0; i < 3; i++) {
        Qi = this.$data.pCharges[i].Q;
        Qx = this.$data.pCharges[i].x;
        Qy = this.$data.pCharges[i].y;
        r = Math.sqrt((Tx - Qx) ** 2 + (Ty - Qy) ** 2);
        Etemp = Qi / (2 * Math.PI * 8.854187 * 10 ** -12 * r ** 2);
        Ex = Ex + Etemp * (Tx - Qx);
        Ey = Ey + Etemp * (Ty - Qy);
      }
      this.$data.Ex = (Math.round(Ex * 1000) / 1000).toFixed(3);
      this.$data.Ey = (Math.round(Ey * 1000) / 1000).toFixed(3);
    },
  },
};
</script>

<style></style>
