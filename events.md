# イベント周り

基本的に getBBox を利用すること。

MouseEvent はブラウザのページの座標が取れてくるが、SVG 内の座標系にそのまま代入してしまうと、viewBox がピクセルと一致していない場合や transform がかかっている場合に座標がずれてしまう。

## Demo

<Draggable/>

ハチャメチャな transform がかかっている場合でも正常にドラッグできる。

<<< @/.vuepress/components/Draggable.vue{2}
