<template>
  <div id="app">
    <header id="header">
      <p>Start Game</p>
      <p>Score 100</p>
    </header>
    <div id="container">
      <NumberBlock v-for="numberBlock in numberBlocks" :key="numberBlock.id" :num="numberBlock.num" :mSlot="slots[numberBlock.slotId]" @click.native="changeSlotId(numberBlock.id)"></NumberBlock>
    </div>
    <!-- <button @click="addNumberBlock">Add New Block</button> -->
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'

import NumberBlock from '@/components/NumberBlock.vue'

export default {
  name: 'App',
  components: {
    NumberBlock
  },
  data() {
    return {
      show: Boolean,
      slots: [],
      numberBlocks: [],
      emptySlots: []
    }
  },
  methods: {
    initData() {
      this.show = true
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          this.slots.push({
            id: i * 4 + j,
            x: j * 100 + 10,
            y: i * 100 + 10,
            isEmpty: true
          })
        }
      }
      for (let i = 0; i < 16; i++) {
        this.emptySlots.push(i)
      }
    },
    move(e) {
      const key = e.key
      switch (key) {
        case 'ArrowUp':
          // console.log(key)
          this.moveToUp()
          this.addNumberBlock()
          break
        case 'ArrowDown':
          // console.log(key)
          this.moveToBottom()
          this.addNumberBlock()
          break
        case 'ArrowLeft':
          // console.log(key)
          this.moveToLeft()
          this.addNumberBlock()
          break
        case 'ArrowRight':
          // console.log(key)
          this.moveToRight()
          this.addNumberBlock()
          break
        default:
          break
      }
    },
    setSlotEmpty(slotToSetEmpty) {
      this.slots[slotToSetEmpty].isEmpty = true
      this.emptySlots.push(slotToSetEmpty)
    },
    setSlotFull(slotToSetFull) {
      this.slots[slotToSetFull].isEmpty = false
      this.emptySlots = this.emptySlots.filter(id => id !== slotToSetFull)
    },
    moveBlock(from, to) {
      this.setSlotEmpty(from)
      this.setSlotFull(to)
      this.numberBlocks.forEach(block => {
        if (block.slotId === from) {
          block.slotId = to
        }
      })
    },
    moveToUp() {
      // console.log('MOVE TO UP')
      // Start from 15 to ensure bolcks at top moved first
      for (let i = 0; i < 16; i++) {
        if (!this.slots[i].isEmpty) {
          let to = i % 4
          while (to < i && !this.slots[to].isEmpty) {
            to += 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToBottom() {
      // console.log('MOVE TO BOTTOM')
      // Start from 15 to ensure bolcks at bottom moved first
      for (let i = 15; i >= 0; i--) {
        if (!this.slots[i].isEmpty) {
          let to = 12 + i % 4
          while (to > i && !this.slots[to].isEmpty) {
            to -= 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToLeft() {
      // console.log('MOVE TO LEFT')
      // Start from 0 to ensure bolcks at left moved first
      for (let i = 0; i < 16; i++) {
        if (!this.slots[i].isEmpty) {
          let to = Math.floor(i / 4) * 4
          while (to < i && !this.slots[to].isEmpty) {
            to += 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToRight() {
      // console.log('MOVE TO RIGHT')
      // Start from 15 to ensure bolcks at right moved first
      for (let i = 15; i >= 0; i--) {
        if (!this.slots[i].isEmpty) {
          let to = Math.floor(i / 4) * 4 + 3
          while (to > i && !this.slots[to].isEmpty) {
            to -= 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    addNumberBlock() {
      if (this.emptySlots.length === 0) {
        return
      }
      const slotId = this.emptySlots[Math.floor(Math.random() * this.emptySlots.length)]
      this.emptySlots = this.emptySlots.filter(id => id !== slotId)
      this.slots[slotId].isEmpty = false
      const num = Math.random() >= 0.7 ? 4 : 2
      this.numberBlocks.push({
        id: uuidv4(),
        slotId: slotId,
        num: num
      })
    },
    removeNumberBlock(id) {
      this.numberBlocks = this.numberBlocks.filter(block => block.slotId !== id)
      this.emptySlots.push(id)
    }
  },
  created() {
    window.addEventListener('keyup', this.move)
    this.initData()
    this.addNumberBlock()
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
  position: relative;
}

button {
  text-align: center;
}

</style>
