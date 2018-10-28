<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
    <table class="minesweeper" >
      <tr v-for="(row, rowIndex) in tiles" :key="rowIndex">
        <TheTile v-for="(tile, colIndex) in row" :key="colIndex" :state="tile" :leftClick="openTile" :rightClick="setFlag"></TheTile>
      </tr>
    </table>
  </div>
</template>

<script>
import TheTile from './components/TheTile.vue';

export default {
  name: 'App',
  components: {
    TheTile,
  },
  data: () => {
    return {
      tiles:[],
      isStart: false,
      isSuccess: false,
      isFailure: false,
    };
  },
  methods: {
    startGame:function(){
      this.init();
      this.tiles = [];
      for(let row=0; row < 10; row+=1) {
        this.tiles.push([]);
        for(let col=0; col < 19; col+=1) {
          this.tiles[row][col] = {
            row: row,
            column: col,
            class: 'unopened',
            mine: Math.random() >= 0.5,
          };
        }
      }
    },
    reset: function() {
      this.init();
    },
    init: function() {
      this.tiles = [];
      this.isStart = false;
      this.isSuccess = false;
      this.isFailure = false;
    },
    /**
     * opens a tile
     * @function
     * @param {Object} tile
     * @return {undefined}
     */
    openTile: function() {
    },

    /**
     * setFlag
     * @function
     * @param {Object} tile
     * @return {undefined}
     */
    setFlag: function(tile) {
      tile.class = 'flagged';
    },
  }
};
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
  background: #333;
  color: #fff;
}

button {
  margin: 20px auto;
  display: block;
}

table.minesweeper {
  margin: auto;
  border: 1px solid #999;
  border-collapse: collapse;
  background-color: #ddd;
}

table.minesweeper td {
  width: 24px;
  height: 24px;
  background-size: cover;
}
</style>
