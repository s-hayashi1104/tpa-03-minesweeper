<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
    <table class="minesweeper" >
      <tr v-for="(row, rowIndex) in tiles" :key="rowIndex">
        <TheTile v-for="(tile, colIndex) in row" :key="colIndex" :state="tile" @leftClick="openTile" @rightClick="setFlag"></TheTile>
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
    openTile: function(tile) {
      if(tile.class === 'opened'  || tile.class === 'flagged') {
        return;
      }
      if (tile.mined) {
        tile.class = 'mined';
        this.isFailure = true;
        this.allOpenTiles();
      }else{
        tile.class = 'opened';
        const row = parseInt(tile.row, 10);
        const col = parseInt(tile.column, 10);
        let arr = [];
        if (row === 0) {
          if (col === 0) {
            arr = [this.tiles[row][col+1],
              this.tiles[row+1][col], this.tiles[row+1][col+1]];
          }
          else if (col === 18) {
            arr = [this.tiles[row][col-1],
              this.tiles[row+1][col-1], this.tiles[row+1][col]];         
          }
          else{
            arr = [this.tiles[row][col-1], this.tiles[row][col+1],
              this.tiles[row+1][col-1], this.tiles[row+1][col], this.tiles[row+1][col+1]];
          }
        }
        else if (row === 9) {
          if (col === 0) {
            arr = [this.tiles[row-1][col], this.tiles[row-1][col+1],
              this.tiles[row][col+1]];
          }
          else if (col === 18) {
            arr = [this.tiles[row-1][col-1], this.tiles[row-1][col],
              this.tiles[row][col-1]];
          }
          else{
            arr = [this.tiles[row-1][col-1], this.tiles[row-1][col],
              this.tiles[row][col-1]];
          }
        }
        else if (col === 0){
          arr = [this.tiles[row-1][col], this.tiles[row-1][col+1],
            this.tiles[row][col+1],
            this.tiles[row+1][col], this.tiles[row+1][col+1]];
        }
        else if (col === 18) {
          arr = [this.tiles[row-1][col-1], this.tiles[row-1][col],
            this.tiles[row][col-1],
            this.tiles[row+1][col-1], this.tiles[row+1][col]];
        }
        else {
          arr = [this.tiles[row-1][col-1], this.tiles[row-1][col], this.tiles[row-1][col+1],
            this.tiles[row][col-1], this.tiles[row][col+1],
            this.tiles[row+1][col-1], this.tiles[row+1][col], this.tiles[row+1][col+1]];
        }

        let flag = true;
        for (let i =0; i < arr.length; i+=1){
          let element = arr[i];
          if(element.mined){
            flag = false;
            break;
          }
        }
        if (flag){
          arr.forEach((tile) =>{
            tile.class = 'opened';
          });
        }
      }
    },
    /**
     * opens all tiles
     * @function
     * @return {undefined}
     */
    allOpenTiles:function(){
      this.tiles.forEach((row) => {
        row.forEach((tile)=> {
          this.openTile(tile);
          return;
        });
      });
    },
    /**
     * setFlag
     * @function
     * @param {Object} tile
     * @return {undefined}
     */
    setFlag: function(tile) {
      if (tile.class === 'flagged') {
        tile.class = 'unopened';
      }
      if (tile.class === 'unopened') {
        tile.class = 'flagged';
      }
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
