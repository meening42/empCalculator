<template>
  <div>
    <ExerciseHeader :exTitle="exTitle" :exText="exText"></ExerciseHeader>
    <pChargeInput v-for="(propCharge, index) in pCharges" 
      :key="index"
      v-bind:propCharge="propCharge"
      v-bind:index="index"
      v-on:charge-changed="onChargeChange">
    </pChargeInput>
    <pointInput v-bind:T="T"  
    v-bind:pCharges="pCharges"
                v-on:point-changed="onPointChange"> 
    </pointInput>
    <button v-on:click="calculateEx1">Izračunaj</button>
    <p>V = {{V}} </p>
    <p>Ex = {{Ex}} Ey = {{Ey}}</p>
  </div>
</template>

<script>
import pChargeInput from '../components/pChargeInput'
import pointInput from '../components/pointInput'
import ExerciseHeader from '../components/ExerciseHeader'


export default {
  name: 'ex1',
  components: { 
    pChargeInput,
    pointInput,
    ExerciseHeader,
  },
  data: function() {
    return{
      pCharges:[
        {
          Q: 1.5e-7,
          x:7,
          y:2
        },
        {
          Q: 1.1e-7,
          x:21,
          y:11
        },
        {
          Q: -1.3e-7,
          x:13,
          y:20
        }
      ],
      V:0,
      Ex:0,
      Ey:0,
      T:{
        x:3,
        y:12
      },
      exTitle:"Naloga 1",
      exText: "Določite V in E. Izvori so tri točkaste elektrine:"
    }
  },
  methods: {
    onChargeChange(name, value, index) {
      this.$data.pCharges[index][name] = value;
    },
    onPointChange(name, value) {
      this.$data.T[name] = value;
    },
    calculateEx1: function () {
      let i,r,Tx,Ty,Qx,Qy,Qi,Etemp,Ex,Ey,Vtemp;
      Tx = this.$data.T.x;
      Ty = this.$data.T.y;
      Vtemp=0;
      Ex = 0;
      Ey = 0;
      for (i = 0; i < 3; i++) {
        Qi = this.$data.pCharges[i].Q;
        Qx = this.$data.pCharges[i].x;
        Qy = this.$data.pCharges[i].y;
        r = Math.sqrt((Tx-Qx)**2+(Ty-Qy)**2);
        Vtemp = Vtemp + Qi/(4*Math.PI*8.854187*(10**(-12))*r)
        Etemp = Qi/(4*Math.PI*8.854187*(10**(-12))*r**3)
        Ex = Ex + Etemp*(Tx-Qx);
        Ey = Ey + Etemp*(Ty-Qy);
      }
      this.$data.V = Vtemp;
      this.$data.Ex = Ex;
      this.$data.Ey = Ey;
    }
  }
}
</script>

<style>
</style>