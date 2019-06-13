<template>
  <div>
    <svg touch-action="none" ref="canv" width="600" height="400" viewBox="0 0 300 300">
      <g transform="scale(2,1) rotate(30,0,0) translate(10,50)">
        <rect
          @pointerdown="onPointerDown"
          @pointermove="onPointerMove"
          @pointerup="onPointerUp"
          :x="p.x"
          :y="p.y"
          width="100"
          height="100"
          fill="#DDD"
          stroke="black"
        ></rect>
        <rect
          v-if="offset"
          :x="p.x"
          :y="p.y"
          :width="offset.x"
          :height="offset.y"
          fill="#9C9"
          style="pointer-events: none"
        ></rect>
      </g>
    </svg>
  </div>
</template>

<script>
function screenToSvg(point, el, svg) {
  const pt = svg.createSVGPoint();
  pt.x = point.x;
  pt.y = point.y;
  return pt.matrixTransform(el.getScreenCTM().inverse());
}

export default {
  data() {
    return {
      p: {
        x: 100,
        y: 100
      },
      offset: null
    };
  },
  methods: {
    onPointerUp(e) {
      this.offset = null;
    },
    onPointerDown(e) {
      const svg = this.$refs.canv;
      const rect = e.target;
      const bbox = e.target.getBBox();
      this.offset = screenToSvg({ x: e.clientX, y: e.clientY }, rect, svg);
      this.offset.x -= bbox.x;
      this.offset.y -= bbox.y;
      rect.setPointerCapture(e.pointerId);
    },
    onPointerMove(e) {
      if (this.offset) {
        const rect = e.target;
        const bbox = e.target.getBBox();
        //SVG要素への参照を取る
        const svg = this.$refs.canv;
        this.p = screenToSvg({ x: e.clientX, y: e.clientY }, rect, svg);
        this.p.x -= this.offset.x;
        this.p.y -= this.offset.y;
      }
    }
  }
};
</script>

<style>
</style>
