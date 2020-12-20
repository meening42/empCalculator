<template>
  <div>
    <ExerciseHeader :exTitle="exTitle" :exText="exText"></ExerciseHeader>
    <finLineCharge
      v-for="(propCharge, index) in pCharges"
      :key="index"
      v-bind:propCharge="propCharge"
      v-bind:index="index"
      v-on:charge-changed="onChargeChange"
    >
    </finLineCharge>
    <pointInput v-bind:T="T" v-on:point-changed="onPointChange"> </pointInput>
    <button v-on:click="calculateEx1">Izračunaj</button>
    <p>V = {{ V }}</p>
    <p>Ex = {{ Ex }} Ey = {{ Ey }}</p>
  </div>
</template>

<script>
import pointInput from "../components/pointInput";
import ExerciseHeader from "../components/ExerciseHeader";
import finLineCharge from "../components/finLineCharge";
const EPS_0 = 8.854 * 10 ** -12;
function getVfinLine(r, z, L, q) {
  let temp, temp1, temp2, Vtemp;
  temp = q / (4 * Math.PI * EPS_0);
  temp1 = z + L / 2 + Math.sqrt(r ** 2 + (z + L / 2) ** 2);
  temp2 = z - L / 2 + Math.sqrt(r ** 2 + (z - L / 2) ** 2);
  Vtemp = temp * Math.log(temp1 / temp2);
  return Vtemp;
}

function getEfinLine(r, z, l, q) {
  console.log("calculate E");
  console.log(Math.atan(1));
  console.log(r, z, l, q);
  let a1, a2;
  let E = { r: 0, z: 0 };
  a1 = Math.PI - Math.atan(r / (l / 2 - z));
  a2 = Math.atan(r / (l / 2 + z));
  console.log("a1 = ", a1, "a2 = ", a2);
  E.r = (q / (4 * Math.PI * EPS_0 * r)) * (Math.cos(a2) - Math.cos(a1));
  E.z = (q / (4 * Math.PI * EPS_0 * r)) * (Math.sin(a1) - Math.sin(a2));
  console.log("Er = ", E.r, "Ez = ", E.z);
  return E;
}

export default {
  name: "ex3",
  components: {
    pointInput,
    ExerciseHeader,
    finLineCharge,
  },
  data: function() {
    return {
      pCharges: [
        {
          q: 1.0e-9,
          x_z: 1,
          x_k: 18,
          y: 1,
        },
        {
          q: -1.2e-9,
          x: 20,
          y_z: 4,
          y_k: 18,
        },
        {
          q: 1.5e-9,
          x_z: 3,
          x_k: 18,
          y: 22,
        },
      ],
      V: 0,
      Ex: 0,
      Ey: 0,
      T: {
        x: 11,
        y: 9,
      },
      exTitle: "3. naloga",
      exText:
        "Določite V in E v točki T. Izvori so tri končno dolge naelektrene palice, ki ležijo v xy ravnini:",
    };
  },
  methods: {
    onChargeChange(name, value, index) {
      console.log(name, index, value);
      this.$data.pCharges[index][name] = value;
    },
    onPointChange(name, value) {
      this.$data.T[name] = value;
    },
    calculateEx1: function() {
      let Tx, Ty, v1, v2, v3; //, Qx,Qy,Qi,Etemp,Ex,Ey;
      let r1, l1, z1, q1;
      let r2, l2, z2, q2;
      let r3, l3, z3, q3;
      Tx = this.$data.T.x;
      Ty = this.$data.T.y;
      q1 = this.$data.pCharges[0].q;
      r1 = Ty - this.$data.pCharges[0].y;
      l1 = this.$data.pCharges[0].x_k - this.$data.pCharges[0].x_z;
      z1 = Tx - (this.$data.pCharges[0].x_z + l1 / 2);
      v1 = getVfinLine(r1, z1, l1, q1);
      q2 = this.$data.pCharges[1].q;
      r2 = this.$data.pCharges[1].x - Tx;
      l2 = this.$data.pCharges[1].y_k - this.$data.pCharges[1].y_z;
      z2 = Ty - (this.$data.pCharges[1].y_z + l2 / 2);
      v2 = getVfinLine(r2, z2, l2, q2);
      q3 = this.$data.pCharges[2].q;
      r3 = this.$data.pCharges[2].y - Ty;
      l3 = this.$data.pCharges[2].x_k - this.$data.pCharges[2].x_z;
      z3 = this.$data.pCharges[2].x_z + l3 / 2 - Tx;
      v3 = getVfinLine(r3, z3, l3, q3);
      this.$data.V = v1 + v2 + v3;
      let E1, E2, E3;
      E1 = getEfinLine(r1, z1, l1, q1);
      E2 = getEfinLine(r2, z2, l2, q2);
      E3 = getEfinLine(r3, z3, l3, q3);
      this.$data.Ex = E1.z - E2.r - E3.z;
      this.$data.Ey = E1.r + E2.z - E3.r;
    },
  },
};
</script>

<style></style>
