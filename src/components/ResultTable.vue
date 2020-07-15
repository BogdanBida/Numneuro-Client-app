<template>
  <div class="wrapper">
      <NumberItem v-for="num in numbersData" :key="num.value" :number="num.value-1" :percent="num.percent"/>
      <div class="waiter" v-if="isWaiting"></div>
  </div>
</template>
<script>
import NumberItem from './NumberItem.vue'
export default {
  name: 'ResultTable',
  components: {
    NumberItem
  },
  props: ['numbersData', 'isWaiting']
}
</script>
<style lang="scss" scoped>
.wrapper {
  border-right: 2px solid #1a5272;
  width: auto;
  border-top: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  padding: 4px;
  padding-top: 32px;
  position: relative;
}

@keyframes bounce {
  from {
    margin-left: 0;
    width: 10%;
  }
  50% {
    width: 20%;
  }
  to {
    width: 10%;
    margin-left: 90%;
  }
}

.waiter {
  position: absolute;
  width: 90%;
  height: 16px;
  top: 8px;
  border: 2px solid whitesmoke;
  border-left: 0;
  border-right: 0;
  &::before {
    position: absolute;
    left: 0;
    top: 0;
    width: 25%;
    height: 100%;
    content: " ";
    background-color: aliceblue;
    animation: bounce 0.5s ease-in-out 0s infinite alternate;
  }
}
</style>
