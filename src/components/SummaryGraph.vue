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
              <text fill="currentColor" y="9" dy="0.71em" :transform="`translate(0, 0)`">{{ month.id }}</text>
            </g>
          </g>
        <!-- axis right -->
          <g class="y-axis" fill="none" :transform="`translate(0, ${-marginBottom})`">
            <path class="domain" stroke="black" :d="`M0.5,${height}.5H0.5V0.5H-6`"></path>
            <g class="tick" opacity="1" font-size="10" font-family="sans-serif" text-anchor="end"
              v-for="(tick, index) in yTicks"
              :key="index"
              :transform="`translate(0, ${y(tick)})`"
              >
              <line stroke="currentColor" x2="-6"></line>
              <text fill="currentColor" x="-9" dy="0.3em">{{ commaFormat(tick) }}</text>
            </g>
          </g>
        <!-- Series -->
          <g class="cy_band" :transform="`translate(0, ${-marginBottom})`">
            <path class="band_cy" :d="cy_band"></path>
          </g>
          <!-- PPY -->
          <g v-if="years.ppy" class="ppy" :transform="`translate(0, ${-marginBottom})`">
            <circle class="dot_ppy"
              v-for="(dot, index) in ppy"
              :key="index"
              :cx="dot.width" :cy="dot.height - marginBottom" r="3" />
            <path class="line_ppy" :d="ppy_line"></path>
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
        loading: false
      }
    },
    mounted() {
		axios
			.get('http://localhost:5000/api/v1/graph')
			.then(response => {
				this.data = response.data.body.items
        console.log(this.data)
			})
			.catch(error => {
				console.log(error)
				this.errored = true
			})
			.finally(() => this.loading = false)
    },
    computed:{
      height(){
        return this.h - this.marginTop -this.marginTop;
      },
      width(){
        return this.w - this.marginLeft -this.marginRight;
      },
      months() {
        let spacer = this.width / 12
        let months = this.data.map(d => {
          return {
            id: d.x,
            x: d.id * spacer,
            y: d.y
          };
        });
        return months;
      },
      y() {
        //Get max value from all series (years)
        let max_values = []
        let y_max_vals = this.data.map(e => e.y)
        let y_max = Math.max(...y_max_vals);
        max_values.push(y_max)

        let values = this.data.map(e => e.y);
        
        let max = (Math.max(...max_values)) *1.2
        let min = (Math.min(...values)) * 0.3
        return d3.scaleLinear()
            .range([0, this.height])
            .domain([max , min])
      },
      yTicks() {
        //let formatComma = d3.format(",")
        let ticks = this.y.ticks(5);
        return ticks
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
      },
      commaFormat: function(value){
        let format = d3.format(",")
        return format(value)
      },
    }
  }
</script>

<style>

</style>