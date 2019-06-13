<template>
  <div>
    <p>NestingSVG</p>

    <svg width="100%" height="300" style="border: 1px solid red;">
      <svg width="50%" height="500" viewBox="0 0 400 400">
        <rect width="50%" height="100"></rect>
        <text x="50%" y="50%">Rect1</text>
        <rect x="10%" y="10%" width="50%" height="100"></rect>
      </svg>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  }
};
</script>

<style>
rect {
  fill: rgba(0, 0, 0, 0.1);
}
</style>
