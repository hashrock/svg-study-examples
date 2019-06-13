<template>
  <div>
    <svg touch-action="none" ref="canv" width="600" height="400" viewBox="0 0 300 300">
      <rect
        @pointerdown="onPointerDown"
        @pointermove="onPointerMove"
        @pointerup="onPointerUp"
        :x="p.x"
        :y="p.y"
        width="200"
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
    </svg>
  </div>
</template>

<script>
function screenToSvg(point, el) {
  const pt = el.createSVGPoint();
  //スクリーン座標を取得
  pt.x = point.x;
  pt.y = point.y;
  const p = pt.matrixTransform(el.getScreenCTM().inverse());
  console.log(p);
  return {
    x: p.x,
    y: p.y
  };
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
      const rect = e.target;
      const bbox = e.target.getBoundingClientRect();
      this.offset = {
        x: e.clientX - bbox.left,
        y: e.clientY - bbox.top
      };
      rect.setPointerCapture(e.pointerId);
    },
    onPointerMove(e) {
      if (this.offset) {
        //SVG要素への参照を取る
        const svg = this.$refs.canv;
        this.p = screenToSvg(
          { x: e.clientX - this.offset.x, y: e.clientY - this.offset.y },
          svg
        );
      }
    }
  }
};
</script>

<style>
</style>
