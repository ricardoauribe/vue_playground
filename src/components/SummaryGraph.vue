<template>
  <div>
    <svg class="lineChart" :width="w" :height="h" v-if="ready">
      <g :transform="`translate(${marginLeft}, ${marginTop})`">
        <!-- Rect to track mouse movement -->
        <g >
          <rect rx="0" ry="0" class="mouse_tracker" @mousemove="track_movement($event)"/>
        </g>
        <!-- axis bottom -->
          <g class="x-axis" fill="none" :transform="`translate(0, ${height - marginBottom})`">
            <path stroke="black" :d="`M0.5,6V0.5H${width}.5V6`"></path>
            <g class="tick" opacity="1" font-size="10" font-family="sans-serif" text-anchor="middle" 
              v-for="(month, index) in months"
              :key="index"
              :transform="`translate(${month.x} , 0)`">
              <line stroke="currentColor" y2="6"></line>
              <text fill="currentColor" y="9" dy="0.71em" :transform="`translate(0, 0)`">{{ month.xLabel }}</text>
            </g>
          </g>
    
      </g>
    </svg>
  </div>
</template>

<script>
  import * as d3 from "d3";
  import axios from 'axios';

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
    mounted() {
		axios
			.get('http://localhost:5000/api/v1/graph')
			.then(response => {
				this.data = response.data.body.items
        console.log("Here is the array")
        console.log(this.data)
			})
			.catch(error => {
				console.log(error)
				this.errored = true
			})
			.finally(() => this.loading = false)
    },
    computed:{
      months() {
        let spacer = this.width / 11
        let months = this.data.map(d => {
          return {
            xLabel: d.x,
            x: d.id * spacer,
            y: d.y
          };
        });
        return months;
      },
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