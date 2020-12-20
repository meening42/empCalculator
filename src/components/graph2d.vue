<template>
  <div>
    <canvas id="myCanvas" v-bind:width="cSize" v-bind:height="cSize">

    </canvas>
  </div>
</template>

<script>

export default{
  data () {
    return {
      graphMin: 0,
      graphMax: 25,
      gridSize: 1,
      cSize:350
    }
  },
  name: "graph2d",
  props:{
    T: Object,
    pCharges: Object
  },
  watch: {
    pCharges: function() { // watch it
      //console.log('Prop changed: ', newVal, ' | was: ', oldVal);
      this.updateGraph();
    },
    T: function() {
      this.updateGraph();
    }
  },
  methods: {
    drawGrid(){
      this.vueCanvas.clearRect(0, 0, this.cSize,this.cSize);
      this.vueCanvas.strokeStyle = "#888";
      this.vueCanvas.beginPath();
      this.vueCanvas.lineTo(this.cSize,this.cSize);
      let gridLines, gridSpacePx;
      gridLines = (this.graphMax - this.graphMin)/this.gridSize;
      gridSpacePx =this.cSize/gridLines;
      this.vueCanvas.moveTo(gridSpacePx, 0);
      for (let i = 1; i < gridLines; i++) {
        this.vueCanvas.lineTo(i*gridSpacePx, this.cSize);
        this.vueCanvas.moveTo((i+1)*gridSpacePx, 0);
      }
      this.vueCanvas.moveTo(0, gridSpacePx);
      for (let i = 1; i < gridLines; i++) {
        this.vueCanvas.lineTo( this.cSize, i*gridSpacePx);
        this.vueCanvas.moveTo(0, (i+1)*gridSpacePx);
      }
      this.vueCanvas.stroke();
    },
    drawPoint(x,y,name,color){
      let graphScalar = this.cSize/(this.graphMax - this.graphMin)
      this.vueCanvas.beginPath();
      this.vueCanvas.moveTo(x*graphScalar, y*graphScalar);
      this.vueCanvas.arc(x*graphScalar, this.cSize-y*graphScalar, 4, 0, 2 * Math.PI);
      this.vueCanvas.fillStyle = color;
      this.vueCanvas.fill();
      this.vueCanvas.fillStyle = "black";
      this.vueCanvas.fillText(name, x*graphScalar+4,this.cSize- y*graphScalar-4);
    },
    updateGraph(){
      this.vueCanvas.clearRect(0, 0, this.cSize, this.cSize);
      this.drawGrid();
      let p;
      for (p in this.pCharges) {
        let indx = parseInt(p)+1;
        this.drawPoint(this.pCharges[p].x,this.pCharges[p].y,"Q"+indx,"#F00");
      } 
      this.drawPoint(this.T.x,this.T.y,"T","#22F");
    }
  },
  mounted() {
      var c = document.getElementById("myCanvas");
      var ctx = c.getContext("2d");    
      this.vueCanvas = ctx;
      this.vueCanvas.font = "15px Arial";
      this.updateGraph();
  }
}

</script>
<style scoped>
  canvas{
    border: 1px solid #000000;
  }

  .cordinate{
    margin-left: 10px;
  }
</style>
