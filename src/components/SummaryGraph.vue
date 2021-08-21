<template>
  <div>
    <svg class="lineChart" :width="w" :height="h" v-if="ready">
      <g :transform="`translate(${marginLeft}, ${marginTop})`">
        <!-- Rect to track mouse movement -->
        <g >
          <rect rx="0" ry="0" class="mouse_tracker" @mousemove="track_movement($event)"/>
        </g>
      
      </g>
    </svg>
  </div>
</template>

<script>
  import * as d3 from "d3";

  export default {
    name: 'SummaryGraph',
    data() {
      return {
        data: [],
        ready: true,
        h: 400,
        w: 560,
        marginLeft: 60,
        marginRight: 10,
        marginTop: 10,
        marginBottom: 20,
      }
    },
    methods: {
      track_movement: function(event){
        const m = d3.pointer(event)
        console.log(m)
        /*
        let myx = this.x.invert(m[0])
        let myy = this.y.invert(m[1])
        let periods = [0,1,2,3,4,5,6,7,8,9,10,11]
        let lineVal = d3.bisectLeft(periods, myx)-1
        this.linex = this.x(lineVal)
        this.tooltip(lineVal)
        */
      }
    }
  }
</script>

<style>

</style>