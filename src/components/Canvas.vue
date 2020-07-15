<template>
  <div class="canv-wrapper">
    <div class="bgr-canvas"> </div>
    <canvas ref="canvas" width="512" height="512">Not support in your brouser or device</canvas>
    <img class="img1" src="../assets/decorative-corner-1.png" alt=" ">
    <img class="img2" src="../assets/decorative-corner-1.png" alt=" ">
  </div>
</template>

<script>
export default {
  name: 'Canvas',
  props: ['isAnalyze', 'isClear'],
  data () {
    return {
      canvas: null,
      ctx: null,
      isDrawing: false,
      startX: 0,
      startY: 0
    }
  },
  mounted () {
    this.canvas = this.$refs.canvas
    this.ctx = this.canvas.getContext('2d')
    this.canvas.width = this.canvas.clientWidth
    this.canvas.height = Math.min(this.canvas.clientHeight, this.canvas.clientWidth)
    this.canvas.addEventListener('mousedown', this.mousedown)
    this.canvas.addEventListener('mousemove', this.mousemove)
    document.addEventListener('mouseup', this.mouseup)
    window.addEventListener('resize', function () {
      this.canvas.width = this.canvas.clientWidth
      this.canvas.height = Math.min(this.canvas.clientHeight, this.canvas.clientWidth)
    })
  },
  methods: {
    mousedown: function (e) {
      var vm = this
      var rect = vm.canvas.getBoundingClientRect()
      var x = e.clientX - rect.left
      var y = e.clientY - rect.top
      vm.isDrawing = true
      vm.startX = x
      vm.startY = y
    },
    mousemove: function (e) {
      var vm = this
      var rect = vm.canvas.getBoundingClientRect()
      var x = e.clientX - rect.left
      var y = e.clientY - rect.top
      if (vm.isDrawing) {
        vm.ctx.beginPath()
        vm.ctx.moveTo(vm.startX, vm.startY)
        vm.ctx.lineTo(x, y)
        vm.ctx.lineWidth = vm.canvas.clientHeight / 12
        vm.ctx.lineCap = 'round'
        vm.ctx.strokeStyle = 'rgba(0,0,0,1)'
        vm.ctx.stroke()
        vm.startX = x
        vm.startY = y
      }
    },
    mouseup: function (e) {
      this.isDrawing = false
    }
  },
  watch: {
    isAnalyze: function (val) {
      if (val) {
        const data = this.canvas.toDataURL()
        this.$emit('is-read', data)
      }
    },
    isClear: function (val) {
      if (val) {
        this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
        this.$emit('is-clear')
      }
    }
  }
}
</script>

<style scoped lang="scss">
  .canv-wrapper {
    width: 50%;
    position: relative;
    margin-bottom: 8px;
    padding: 8px;
  }
  .bgr-canvas {
    position: absolute;
    top: 8px;
    left: 8px;
    width: calc(100% - 16px);
    height: calc(100% - 16px);
    box-shadow: -2px 5px 10px black, inset 1px -1px 12px 1px rgba(255,255,255,0.2);
    border-radius: 8px;
    backdrop-filter: blur(2px);
    -webkit-backdrop-filter: blur(2px);
    z-index: 2;
  }
  canvas {
    width: 100%;
    z-index: 3;
    opacity: 0.9;
    filter: drop-shadow(1px 1px 3px skyblue);
    cursor: crosshair;
  }

  img {
    position: absolute;
    z-index: 4;
    width: 96px;
    height: 96px;
    filter: brightness(0.95) drop-shadow(-1px 2px 8px black);
    pointer-events: none;
  }

  .img1 {
    left: 0;
    bottom: 0;
  }

  .img2 {
    top: 0;
    right: 0;
    transform: scale(-1);
  }
  @media screen and (max-width: 800px) {
    img {
      width: 72px;
      height: 72px;
    }
  }

  @media screen and (max-width: 640px) {
    .canv-wrapper {
      width: 90%;
    }
    img {
      width: 64px;
      height: 64px;
    }
  }

</style>
