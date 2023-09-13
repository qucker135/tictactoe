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
      <button type="button" @click="resetGame" :disabled="selGameMode==='0'">New Game</button>
    </div>
    <div class="game-board" v-if="gameMode!=='0'">
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
    <div class="legend">
      <div class="legend-option" v-if="gameMode==='1'">
        <div class="legend-option-row">
          <img src="@/assets/cross.png" alt="cross" width="30"/>
          <span> - Player 1</span>
        </div>
        <div class="legend-option-row">
          <img src="@/assets/nough.png" alt="nough" width="30"/>
          <span> - Player 2</span>
        </div>
      </div>
      <div class="legend-option" v-if="gameMode==='2'">
        <div class="legend-option-row">
          <img src="@/assets/cross.png" alt="cross" width="30"/>
          <span> - Player</span>
        </div>
        <div class="legend-option-row">
          <img src="@/assets/nough.png" alt="nough" width="30"/>
          <span> - AI</span>
        </div>
      </div>
      <div class="legend-option" v-if="gameMode==='3'">
        <div class="legend-option-row">
          <img src="@/assets/cross.png" alt="cross" width="30"/>
          <span> - AI</span>
        </div>
        <div class="legend-option-row">
          <img src="@/assets/nough.png" alt="nough" width="30"/>
          <span> - Player</span>
        </div>
      </div>
    </div>
    <div class="turn" v-if="gameMode!=='0'">
      <span>Turn:</span>
      <img :src="whoseRound==='x' ? require('@/assets/cross.png') : require('@/assets/nough.png')" width="50"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GamePanel',
  data() {
    return {
      selGameMode: "0",
      gameMode: "0",
      whoseRound: 'x',
      gameBoard: [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ],
      gameOn: true
    }
  },
  methods: {
    makeMove(row, col) {
      if(this.gameOn) {
        if (this.gameBoard[row][col] === '') {
          this.gameBoard[row][col] = this.whoseRound
          this.whoseRound = this.whoseRound === 'x' ? 'o' : 'x'
        }
        // check if game is over
        if (this.checkIfGameOver()) {
          this.gameOn = false;
        }
      }
    },
    checkIfGameOver() {
      return this.checkIfWin() || this.checkIfDraw();
    },
    checkIfWin() {
      let result = false;
      // check rows
      for (let i = 0; i < 3; i++) {
        if (this.gameBoard[i][0] === this.gameBoard[i][1] && this.gameBoard[i][1] === this.gameBoard[i][2] && this.gameBoard[i][0] !== '') {
          result = true;
        }
      }
      // check columns
      for (let i = 0; i < 3; i++) {
        if (this.gameBoard[0][i] === this.gameBoard[1][i] && this.gameBoard[1][i] === this.gameBoard[2][i] && this.gameBoard[0][i] !== '') {
          result = true;
        }
      }
      // check diagonals
      if (this.gameBoard[0][0] === this.gameBoard[1][1] && this.gameBoard[1][1] === this.gameBoard[2][2] && this.gameBoard[0][0] !== '') {
        result = true;
      }
      if (this.gameBoard[0][2] === this.gameBoard[1][1] && this.gameBoard[1][1] === this.gameBoard[2][0] && this.gameBoard[0][2] !== '') {
        result = true;
      }
      if(result) {
        alert("Player " + (this.whoseRound === 'x' ? 'o' : 'x') + " won!");
      }
      return result;
    },
    checkIfDraw() {
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          if (this.gameBoard[i][j] === '') {
            return false;
          }
        }
      }
      alert('Draw!');
      return true;
    },
    resetGame() {
      this.gameBoard = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ];
      this.whoseRound = 'x';
      this.gameMode = this.selGameMode;
      this.gameOn = true;
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
  .legend-option-row {
    margin-bottom: 5px;
  }
  .legend-option-row img,
  .legend-option-row span,
  .turn span,
  .turn img {
    display: inline-block;
    vertical-align: middle;
  }
  .legend-option-row img {
    margin-right: 5px;
  }
  .turn img{
    margin-left: 5px;
  }
</style>
