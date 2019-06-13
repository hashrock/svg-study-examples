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
