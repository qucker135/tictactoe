<template>
  <div class="panel">
    <div class="controls">
      <select name="modes" id="modes" v-model="selGameMode" @change="debugSelected">
        <option value="0" hidden>Choose Game Mode</option>
        <option value="1">2 players</option>
        <option value="2">Player vs AI (player starts)</option>
        <option value="3">Player vs AI (AI starts)</option>
      </select>
    </div>
    <div class="controls">
      <button type="button" @click="resetGame" :disabled="selGameMode===0">New Game</button>
    </div>
    <div class="game-board">
      <div class="game-row">
        <div class="game-cell" @click="makeMove(0, 0)">
          <img v-if="['x', 'o'].includes(gameBoard[0][0])" :src="getPhoto(0, 0)"/>
        </div>
        <div class="game-cell" @click="makeMove(0, 1)">
          <img v-if="['x', 'o'].includes(gameBoard[0][1])" :src="getPhoto(0, 1)"/>
        </div>
        <div class="game-cell" @click="makeMove(0, 2)">
          <img v-if="['x', 'o'].includes(gameBoard[0][2])" :src="getPhoto(0, 2)"/>
        </div>
      </div>
      <div class="game-row">
        <div class="game-cell" @click="makeMove(1, 0)">
          <img v-if="['x', 'o'].includes(gameBoard[1][0])" :src="getPhoto(1, 0)"/>
        </div>
        <div class="game-cell" @click="makeMove(1, 1)">
          <img v-if="['x', 'o'].includes(gameBoard[1][1])" :src="getPhoto(1, 1)"/>
        </div>
        <div class="game-cell" @click="makeMove(1, 2)">
          <img v-if="['x', 'o'].includes(gameBoard[1][2])" :src="getPhoto(1, 2)"/>
        </div>
      </div>
      <div class="game-row">
        <div class="game-cell" @click="makeMove(2, 0)">
          <img v-if="['x', 'o'].includes(gameBoard[2][0])" :src="getPhoto(2, 0)"/>
        </div>
        <div class="game-cell" @click="makeMove(2, 1)">
          <img v-if="['x', 'o'].includes(gameBoard[2][1])" :src="getPhoto(2, 1)"/>
        </div>
        <div class="game-cell" @click="makeMove(2, 2)">
          <img v-if="['x', 'o'].includes(gameBoard[2][2])" :src="getPhoto(2, 2)"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GamePanel',
  data() {
    return {
      selGameMode: 0,
      gameMode: 0,
      whoseRound: 'x',
      gameBoard: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
    }
  },
  methods: {
    makeMove(row, col) {
      if (this.gameBoard[row][col] === '') {
        this.gameBoard[row][col] = this.whoseRound
        this.whoseRound = this.whoseRound === 'x' ? 'o' : 'x'
      }
    },
    resetGame() {
      this.gameBoard = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ];
      this.whoseRound = 'x';
      this.gameMode = this.selGameMode;
    },
    getPhoto(row, col) {
      if (this.gameBoard[row][col] === 'x') {
        return require('@/assets/cross.png');
      } else if (this.gameBoard[row][col] === 'o') {
        return require('@/assets/nough.png');
      }
    },
    debugSelected() {
      console.log(this.selGameMode);
      console.log(this.gameMode);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .controls {
    margin-bottom: 15px;
  }
  .game-board {
    display: flex;
    flex-direction: column;
    margin-bottom: 15px;
    align-items: center;
  }
  .game-row {
    display: flex;
    flex-direction: row;
  }
  .game-cell {
    width: 100px;
    height: 100px;
    border: 1px solid #000;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 50px;
    color: #000;
    background-color: #fff;
  }
  .game-cell img{
    width: 100px;
    height: auto;
  }
</style>
