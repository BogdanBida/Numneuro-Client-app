<template>
  <div id="app">
    <div class="left">
      <Canvas :isAnalyze="isAnalyze" :isClear="isClear" @is-read="afterReading($event)" @is-clear="afterClear($event)"/>
      <Controller @start-reading="read" @clear="clear"/>
    </div>
    <ResultTable :numbersData="numbers" :isWaiting="waiting"/>
    <div class="bgr bgr1"> </div>
    <div class="bgr bgr2"> </div>
  </div>
</template>

<script>
import Canvas from './components/Canvas.vue'
import ResultTable from './components/ResultTable.vue'
import Controller from './components/Controller.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Canvas,
    ResultTable,
    Controller
  },
  data: () => {
    return {
      isAnalyze: false,
      isClear: false,
      waiting: false,
      numbers: [
        { value: 1, percent: 0 },
        { value: 2, percent: 0 },
        { value: 3, percent: 0 },
        { value: 4, percent: 0 },
        { value: 5, percent: 0 },
        { value: 6, percent: 0 },
        { value: 7, percent: 0 },
        { value: 8, percent: 0 },
        { value: 9, percent: 0 },
        { value: 0, percent: 0 }
      ]
    }
  },
  methods: {
    read: function () {
      this.isAnalyze = true
    },
    clear: function () {
      this.isClear = true
    },
    afterReading: function (data) {
      this.isAnalyze = false
      this.waiting = true
      axios.post('http://localhost:8081/api/brain', { body: data }).then(response => {
        this.waiting = false
        this.numbers = response.data.data
      }).catch(error => {
        console.log(error)
      }).finally(() => {
        this.waiting = false
      })
    },
    afterClear: function () {
      this.isClear = false
      for (let i = 0; i < this.numbers.length; i++) {
        this.numbers[i].percent = 0
      }
    }
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
  width: 100%;
  position: relative;
  min-height: 100vh;
  display: flex;
  background: linear-gradient(to right bottom, #101010, #2b3644, #1f4768, #151d25, #101010);
}

.bgr {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: -40;
  background-image: url('./assets/bgr-x.png');
  background-repeat: repeat;
}

.bgr1 {
  background-size: 8px;
  opacity: 0.5;
}

.bgr2 {
  background-size: 128px;
  opacity: 0.1;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: white;
  display: flex;
  justify-content: space-around;
  align-items: flex-start;
  width: 100%;
}
.left {
  width: 75%;
  height: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

@media screen and (max-width: 640px) {
  .left {
    flex-direction: column;
    justify-content: center;
  }
}
</style>
