<template>
  <div id="app">
    <header id="header">
      <p id="title">2048</p>
      <p id="score">Score: {{ score }}</p>
    </header>
    <div id="container">
      <div id="shade" v-if="gameOver">GAME OVER</div>
      <NumberBlock
        v-for="block in blocks"
        :key="block.id"
        :num="slotBlocks[block.slotId].num"
        :mSlot="SLOTS_INFO[block.slotId]"
        :show="block.show"
      ></NumberBlock>
    </div>
    <button
      id="new-game-button"
      v-if="gameOver"
      @click="newGame"
    >New Game</button>
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
      shouldAddBlock: Boolean,
      SLOTS_INFO: [],
      blocks: [],
      emptySlots: [],
      slotBlocks: [],
      score: Number,
      gameOver: Boolean
    }
  },
  methods: {
    initData() {
      this.gameOver = false
      this.score = 0
      this.blocks = []
      this.slotBlocks = []
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          this.SLOTS_INFO.push({
            x: j * 100 + 10,
            y: i * 100 + 10
          })
        }
      }
      for (let i = 0; i < 16; i++) {
        this.emptySlots.push(i)
        this.slotBlocks.push({
          blockId: '',
          hasBlock: false,
          num: 0
        })
      }
    },
    move(e) {
      if (this.gameOver) {
        return
      }
      const key = e.key
      // Should only add one block for each move
      this.shouldAddBlock = false
      switch (key) {
        case 'ArrowUp':
          this.moveAction(this.moveToUp, this.mergeToUp)
          break
        case 'ArrowDown':
          this.moveAction(this.moveToBottom, this.mergeToBottom)
          break
        case 'ArrowLeft':
          this.moveAction(this.moveToLeft, this.mergeToLeft)
          break
        case 'ArrowRight':
          this.moveAction(this.moveToRight, this.mergeToRight)
          break
      }
    },
    moveAction(moveFunction, mergeFunction) {
      moveFunction()
      setTimeout(() => {
        mergeFunction()
        moveFunction()
        if (this.shouldAddBlock) {
          setTimeout(() => {
            this.addNumberBlock()
            if (this.emptySlots.length === 0 && this.checkGameOver()) {
              this.gameOver = true
            }
          }, 100)
        }
      }, 80)
    },
    setSlotEmpty(slotId) {
      this.slotBlocks[slotId] = {
        blockId: '',
        hasBlock: false,
        num: 0
      }
      this.emptySlots.push(slotId)
    },
    setSlotFull(slotFrom, slotTo) {
      this.emptySlots = this.emptySlots.filter(id => id !== slotTo)
      this.slotBlocks[slotTo] = this.slotBlocks[slotFrom]
    },
    moveBlock(from, to) {
      this.setSlotFull(from, to)
      this.setSlotEmpty(from)
      this.blocks.forEach(block => {
        if (block.slotId === from) {
          block.slotId = to
        }
      })
      this.shouldAddBlock = true
    },
    moveToUp() {
      // Start from 0 to ensure bolcks at top moved first
      for (let i = 0; i < 16; i++) {
        if (this.slotBlocks[i].hasBlock) {
          let to = i % 4
          while (to < i && this.slotBlocks[to].hasBlock) {
            to += 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToBottom() {
      // Start from 15 to ensure bolcks at bottom moved first
      for (let i = 15; i >= 0; i--) {
        if (this.slotBlocks[i].hasBlock) {
          let to = 12 + i % 4
          while (to > i && this.slotBlocks[to].hasBlock) {
            to -= 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToLeft() {
      // Start from 0 to ensure bolcks at left moved first
      for (let i = 0; i < 16; i++) {
        if (this.slotBlocks[i].hasBlock) {
          let to = Math.floor(i / 4) * 4
          while (to < i && this.slotBlocks[to].hasBlock) {
            to += 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToRight() {
      // Start from 15 to ensure bolcks at right moved first
      for (let i = 15; i >= 0; i--) {
        if (this.slotBlocks[i].hasBlock) {
          let to = Math.floor(i / 4) * 4 + 3
          while (to > i && this.slotBlocks[to].hasBlock) {
            to -= 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    mergeToUp() {
      for (let i = 0; i < (16 - 4); i++) {
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i + 4].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i + 4].num) {
          this.removeBlock(i + 4)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
          this.score += this.slotBlocks[i].num
        }
      }
    },
    mergeToBottom() {
      for (let i = 15; i >= 4; i--) {
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i - 4].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i - 4].num) {
          this.removeBlock(i - 4)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
          this.score += this.slotBlocks[i].num
        }
      }
    },
    mergeToLeft() {
      for (let i = 0; i < 16; i++) {
        if (i % 4 === 3) continue
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i + 1].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i + 1].num) {
          this.removeBlock(i + 1)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
          this.score += this.slotBlocks[i].num
        }
      }
    },
    mergeToRight() {
      for (let i = 15; i >= 0; i--) {
        if (i % 4 === 0) continue
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i - 1].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i - 1].num) {
          this.removeBlock(i - 1)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
          this.score += this.slotBlocks[i].num
        }
      }
    },
    addNumberBlock() {
      if (this.emptySlots.length === 0) {
        return false
      }
      const slotId = this.emptySlots[Math.floor(Math.random() * this.emptySlots.length)]
      this.emptySlots = this.emptySlots.filter(id => id !== slotId)
      // this.slotBlocks[slotId].hasBlock = true
      const num = Math.random() >= 0.7 ? 4 : 2
      const id = uuidv4()
      this.blocks.push({
        id: id,
        slotId: slotId,
        show: false
      })
      this.slotBlocks[slotId] = {
        blockId: id,
        hasBlock: true,
        num: num
      }
      setTimeout(() => {
        this.blocks[this.blocks.length - 1].show = true
      }, 100)
      return true
    },
    removeBlock(slotId) {
      this.setSlotEmpty(slotId)
      this.blocks = this.blocks.filter(block => block.slotId !== slotId)
      // Should add new block if there has blocks merged
      this.shouldAddBlock = true
    },
    checkGameOver() {
      for (let i = 0; i < 16; i++) {
        if (i % 4 !== 3 && this.slotBlocks[i].num === this.slotBlocks[i + 1].num) {
          return false
        }
        if (i + 4 < 16 && this.slotBlocks[i].num === this.slotBlocks[i + 4].num) {
          return false
        }
      }
      return true
    },
    newGame() {
      this.initData()
      this.addNumberBlock()
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
    text-align: center;
    font-family: Arial, Helvetica, sans-serif;
  }
  #title {
    font-size: 70px;
    font-weight: bold;
    margin-bottom: 10px;
  }
  #score {
    font-size: 50px;
    margin-top: 10px;
  }
}

#container {
  width: 400px;
  height: 400px;
  background-color: rgba($color: #CCCCCC, $alpha: 0.7);
  border-radius: 10px;
  margin-left: auto;
  margin-right: auto;
  position: relative;
}

#shade {
  position: absolute;
  width: 400px;
  height: 400px;
  background-color: rgba($color: #333333, $alpha: 0.8);
  border-radius: 10px;
  z-index: 99;
  color: white;
  text-align: center;
  line-height: 360px;
  font-size: 42px;
  font-weight: 700;
  font-family: Arial, Helvetica, sans-serif;
}

#new-game-button {
  display: block;
  margin-top: 40px;
  margin-left: auto;
  margin-right: auto;
  padding: 10px;
  color: white;
  font-size: 21px;
  font-weight: bold;
  text-align: center;
  background-color: transparent;
  border-width: 10px;
  border-radius: 10px;
  border-color: white;
}

</style>
