<template>
<div :class="className">{{ num }}</div>
</template>

<script>
export default {
  name: 'NumberBlock',
  props: {
    num: {
      typeof: Number,
      default: 2
    }
  },
  data() {
    return {
      className: String
    }
  },
  created() {
    switch (this.num) {
      case 2:
        this.className = 'two'
        break
      case 4:
        this.className = 'four'
        break
      default:
        this.className = 'two'
    }
  }
}
</script>

<style lang="scss" scoped>
div {
  width: 80px;
  height: 80px;
  border-radius: 10px;
  color: white;
  text-align: center;
  font-size: 50px;
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  margin-top: 10px;
  margin-left: 10px;
  line-height: 80px;
}

.two {
  background-color: orange;
}

.four {
  background-color: yellowgreen;
}
</style>
