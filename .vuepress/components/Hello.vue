<template>
  <div>
    <svg width="300" height="300" ref="canv" viewBox="0 0 300 300">
      <path :d="os" id="path1" />
      <text>
        <textPath font-size="20" href="#path1">うねうねテキストうねうねテキスト</textPath>
      </text>
      <g v-for="item in o">
        <line
          :x1="item.p[0].x"
          :y1="item.p[0].y"
          :x2="item.p[1].x"
          :y2="item.p[1].y"
          stroke="black"
        />
        <line
          :x1="item.p[2].x"
          :y1="item.p[2].y"
          :x2="item.p[3].x"
          :y2="item.p[3].y"
          stroke="black"
        />
      </g>
      <g v-for="item in o">
        <g
          @pointerdown="onPointerDown($event, point)"
          @pointermove="onPointerMove"
          @pointerup="onPointerUp"
          v-for="point in item.p"
          :transform="translate(point.x,point.y)"
        >
          <circle r="10" x="0" y="0" fill="rgba(0,255,0,0.0)" />
          <circle r="5" x="0" y="0" fill="rgba(0,0,0,0.5)" />
        </g>
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
  props: {
    msg: String
  },
  data() {
    return {
      o: [
        {
          type: "C",
          p: [
            { x: 200, y: 200 },
            { x: 240, y: 200 },
            { x: 210, y: 250 },
            { x: 250, y: 250 }
          ]
        }
      ],
      offset: null,
      selection: null
    };
  },
  methods: {
    translate(x, y) {
      return `translate(${x},${y})`;
    },

    onPointerUp(e) {
      this.offset = null;
    },
    onPointerDown(e, item) {
      const rect = e.target;
      const bbox = rect.getBBox();
      this.offset = screenToSvg(
        { x: e.clientX, y: e.clientY },
        rect,
        this.$refs.canv
      );
      rect.setPointerCapture(e.pointerId);
      this.selection = item;
    },
    onPointerMove(e) {
      if (this.offset) {
        let p = screenToSvg(
          { x: e.clientX, y: e.clientY },
          this.$refs.canv,
          this.$refs.canv
        );
        this.selection.x = p.x - this.offset.x;
        this.selection.y = p.y - this.offset.y;
      }
    }
  },
  computed: {
    os() {
      return this.o
        .map(i => {
          if (i.type === "C") {
            return `M ${i.p[0].x},${i.p[0].y} C ${i.p[1].x},${i.p[1].y} ${i.p[2].x},${i.p[2].y} ${i.p[3].x},${i.p[3].y}`;
          }

          // return `${i.type} ${i.p.map(ip => `${ip.x}, ${ip.y}`).join(" ")}`;
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
