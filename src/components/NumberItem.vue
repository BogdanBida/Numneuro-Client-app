<template>
  <div class="container">
    <div class="num">{{number}}</div>
    <div class="percent-wrapper">
      <span>{{roundedPercent}}%</span>
      <div class="bar" v-bind:style="{width : roundedPercent+'%'}"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'NumberItem',
  props: ['number', 'percent'],
  computed: {
    roundedPercent () {
      return Math.round(100 * this.percent)
    }
  }
}
</script>
<style lang="scss" scoped>
.container {
  position: relative;
  display: flex;
  align-items: center;
  padding: 4px;
  margin-bottom: 4px;
  border-radius: 4px;
  min-width: 72px;
  cursor: default;
  left: 0;
  transition: 0.4s;
  > div {
    width: 50%;
    min-width: 16px;
  }
  &:nth-child(odd) {
    background-color: #25668bb9;
  }
  &:hover {
    left: -4px;
    filter: brightness(1.1);
  }
}

.num {
  font-weight: bold;
}

.percent-wrapper {
  position: relative;
  padding: 4px;
  span {
    position: relative;
    z-index: 2;
  }
  .bar {
    transition: 0.2s;
    position: absolute;
    z-index: 0;
    top: 0;
    left: 0;
    height: 100%;
    background-color: rgb(151, 0, 0);
  }
}
</style>
