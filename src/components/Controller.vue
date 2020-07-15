<template>
  <div class="container">
      <button @click="analyze">Analyze</button>
      <button @click="clear">Clear</button>
      <div class="left-border"> </div>
      <div class="bottom-border"> </div>
      <div class="right-border"> </div>
      <div class="top-border"> </div>
  </div>
</template>
<script>
export default {
  name: 'Controller',
  components: {
  },
  methods: {
    analyze: function () {
      this.$emit('start-reading')
    },
    clear: function () {
      this.$emit('clear')
    }
  }
}
</script>
<style lang="scss" scoped>
    .container {
        width: 33.33%;
        display: flex;
        padding: 4px;
        box-sizing: border-box;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        button {
            width: 100%;
        }
        position: relative;
        &:hover {
          button {
            margin: 8px 0;
          }
        }
    }

    @keyframes hover-effect {
      from {
        filter: brightness(1);
      }
      to {
        filter: brightness(1.05);
      }
    }

    button {
      animation: hover-effect 0.6s ease-in-out 0s 0 alternate;
      position: relative;
      text-transform: uppercase;
      letter-spacing: 0.4em;
      font-size: 108%;
      background-color: #1a5272;
      border: 0;
      padding: 8px 4px;
      margin: 4px 0;
      color: white;
      transition: 0.2s;
      cursor: pointer;
      &:hover {
        animation-iteration-count: infinite;
      }
      &:active {
        transition: 0s;
        filter: brightness(2.5) drop-shadow(0 0 12px #55c1ff);
      }
    }

    .left-border, .bottom-border, .right-border, .top-border {
      position: absolute;
      z-index: 2;
      background-color: whitesmoke;
    }

    .left-border {
      left: -12px;
      top: 0;
      width: 2px;
      height: 75%;
      &::before {
        position: absolute;
        top: 0;
        content: " ";
        width: 2px;
        height: 2px;
        box-sizing: border-box;
        border: 8px solid transparent;
        border-left: 8px solid whitesmoke;
        border-top: 8px solid whitesmoke;
      }
    }

    .bottom-border {
      left: 16px;
      bottom: 0;
      width: 100%;
      height: 2px;
       &::before {
        position: absolute;
        content: " ";
        right: 0;
        bottom: 0;
        width: 2px;
        height: 2px;
        box-sizing: border-box;
        border: 8px solid transparent;
        border-right: 8px solid whitesmoke;
        border-bottom: 8px solid whitesmoke;
      }
    }

    .right-border {
      right: -16px;
      width: 2px;
      height: 50%;
    }

    .top-border {
      left: 10%;
      top: 0;
      width: 66.66%;
      height: 2px;
      &::before {
         position: absolute;
        content: " ";
        right: -33.33%;
        bottom: 0;
        width: 25%;
        height: 2px;
        background-color: whitesmoke;
      }
    }

    @media screen and (max-width: 640px) {
        .container {
            width: 75%;
            margin-top: 16px;
        }
    }
</style>
