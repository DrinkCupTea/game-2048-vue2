<template>
  <div id="app">
    <header id="header">
      <p>Start Game</p>
      <p>Score 100</p>
    </header>
    <div id="container">
      <NumberBlock></NumberBlock>
    </div>
  </div>
</template>

<script>
import NumberBlock from '@/components/NumberBlock.vue'

export default {
  name: 'App',
  components: {
    NumberBlock
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
  }
}
</script>

<style lang="scss">

html,body {
  margin: 0;
  padding: 0;
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
}

</style>
