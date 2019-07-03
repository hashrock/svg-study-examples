<template>
  <div class="parent">
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
/**
 * clientX / Y（スクリーン座標）を element内の座標系に変換する
 */
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
      //Rect内のどこがクリックされたか取得
      const rect = e.target;
      const bbox = rect.getBBox();
      this.offset = screenToSvg(
        { x: e.clientX, y: e.clientY },
        rect,
        this.$refs.canv
      );
      this.offset.x -= bbox.x;
      this.offset.y -= bbox.y;
      //領域外のMouseEventもキャプチャする
      rect.setPointerCapture(e.pointerId);
    },
    onPointerMove(e) {
      if (this.offset) {
        this.p = screenToSvg(
          { x: e.clientX, y: e.clientY },
          e.target,
          this.$refs.canv
        );
        //Rect内の掴んだ位置分原点を移動
        this.p.x -= this.offset.x;
        this.p.y -= this.offset.y;
      }
    }
  }
};
</script>

<style>
.child {
  width: 200px;
  height: 200px;
  background: green;
  transform: rotateY(45deg);
}
</style>
