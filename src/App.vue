<template>
  <div id="app">
    <header id="header">
      <p>Start Game</p>
      <p>Score 100</p>
    </header>
    <div id="container">
      <NumberBlock :show="show" :num="num"></NumberBlock>
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
      show: Boolean
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
    }
  },
  created() {
    window.addEventListener('keyup', this.move)
    this.num = 2
    this.show = true
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
  padding-top: 10px;
  padding-left: 10px;
}

button {
  text-align: center;
}

</style>
