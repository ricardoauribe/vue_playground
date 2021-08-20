<template>
  <div>
    <svg class="lineChart" :width="w" :height="h" v-if="ready">
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
        h: 400,
        w: 560,
      }
    }
  }
</script>

<style>

</style>