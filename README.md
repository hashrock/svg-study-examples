# TODO

- SVG Path の編集機能を作ってみる
- textpath の折返し対応を検証する
- CSS 3D Transform をつけた状態で座標計算がずれないか検討

# 基礎

SVG の細かな仕様は座標系と切り離せないんで、練習しながら覚えたい

## SFC として埋め込んでみる

<Hello msg="Wooo"/>

<<< @/.vuepress/components/Hello.vue{2}

# 座標系

## SVG のネスト

[https://www.sarasoueidan.com/blog/nesting-svgs/#nesting-svgs](https://www.sarasoueidan.com/blog/nesting-svgs/#nesting-svgs)

<NestingSvg />

- SVG のネストは viewBox と preserveAspectRatio を SVG 内で使いたくなったときに便利らしい。

## getScreenCTM
