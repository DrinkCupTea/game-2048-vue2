<template>
  <div id="app">
    <header id="header">
      <p>Start Game</p>
      <p>Score 100</p>
    </header>
    <div id="container">
      <NumberBlock v-for="i in 16" :key="i" :show="show" :num="num" :ix="slots[i].x" :iy="slots[i].y"></NumberBlock>
      <!-- <NumberBlock v-for="i in 16" :key="i" :show="show" :num="num" :ix="i * 100" :iy="i * 10"></NumberBlock> -->
    </div>
    <button @click="show = !show">Test Appeaer</button>
    <!-- <button @click="testAppear">Test Move</button> -->
  </div>
</template>

<script>
import NumberBlock from '@/components/NumberBlock.vue'

export default {
  name: 'App',
  components: {
    NumberBlock
  },
  data() {
    return {
      num: Number,
      show: Boolean,
      slots: [],
      emptySlots: []
    }
  },
  methods: {
    move(e) {
      const key = e.key
      switch (key) {
        case 'ArrowUp':
          console.log(key)
          break
        case 'ArrowDown':
          console.log(key)
          break
        case 'ArrowLeft':
          console.log(key)
          break
        case 'ArrowRight':
          console.log(key)
          break
        default:
          break
      }
    },
    init() {
      this.num = 2
      this.show = true
      for (let i = 1; i <= 4; i++) {
        for (let j = 1; j <= 4; j++) {
          this.slots[(j - 1) * 4 + i] = {
            x: (j - 1) * 100 + 10,
            y: (i - 1) * 100 + 10,
            empty: false
          }
        }
      }
    }
  },
  created() {
    window.addEventListener('keyup', this.move)
    this.init()
  }
}
</script>

<style lang="scss">

body {
  margin: 0;
}

#app {
  background-color: cadetblue;
  width: 100%;
  height: 100%;
  position: fixed;
}

#header {
  p {
    color: white;
    font-size: 35px;
    text-align: center;
  }
}

#container {
  width: 400px;
  height: 400px;
  background-color: rgba($color: #EEEEEE, $alpha: 0.7);
  border-radius: 10px;
  margin-left: auto;
  margin-right: auto;
  // padding-top: 10px;
  // padding-left: 10px;
  position: relative;
}

button {
  text-align: center;
}

</style>
