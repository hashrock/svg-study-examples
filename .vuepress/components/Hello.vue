<template>
  <div>
    <svg width="300" height="300" viewBox="0 0 300 300">
      <path :d="os"></path>
      <g v-for="item in o">
        <circle
        @pointermove=""
        
         v-for="point in item.p" r="5" :cx="point.x" :cy="point.y"></circle>
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  },
  data() {
    return {
      d: "M 200,200 C 240,200 210,250 250,250",
      o: [
        { type: "M", p: [{ x: 200, y: 200 }] },
        {
          type: "C",
          p: [{ x: 240, y: 200 }, { x: 210, y: 250 }, { x: 250, y: 250 }]
        }
      ]
    };
  },
  computed: {
    os() {
      return this.o
        .map(i => {
          return `${i.type} ${i.p.map(ip => `${ip.x}, ${ip.y}`).join(" ")}`;
        })
        .join(" ");
    }
  }
};
</script>

<style>
path {
  fill: none;
  stroke: blue;
  stroke-width: 3;
}
</style>
