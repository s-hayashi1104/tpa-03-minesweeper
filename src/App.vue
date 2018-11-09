<template>
  <div id="app">
    <button @click="startGame">Start Game</button>
    <table class="minesweeper" >
      <tr v-for="(row, rowIndex) in tiles" :key="rowIndex">
        <TheTile v-for="(tile, colIndex) in row" :key="colIndex" :state="tile" :className="tile.class" @leftClick="openTile" @rightClick="setFlag"></TheTile>
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
      neighbour: ['mine-neighbor-1', 'mine-neighbor-2', 'mine-neighbor-3', 'mine-neighbor-4', 'mine-neighbor-5', 'mine-neighbor-6',
        'mine-neighbor-7', 'mine-neighbor-8', 'mine-neighbor-9'],
    };
  },
  methods: {
    startGame:function(){
      this.init();
      this.tiles = [];
      for(let row=0; row < 10; row+=1) {
        this.tiles.push([]);
        for(let col=0; col < 19; col+=1) {
          this.tiles[row].push({
            row: row,
            column: col,
            class: 'unopened',
            mine: Math.random() >= 0.7,
          });
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
      if (tile.mine) {
        tile.class = 'mine';
        this.isFailure = true;
        this.allOpenTiles();
      }else{
        let neighbourMines = this.countNeighbourMines(tile);
        if (neighbourMines === 0) {
          tile.class = 'opened';
          this.neighbours(tile).forEach((around) => {
            this.openTile(around);
          });
        }
        else{
          tile.class = `mine-neighbor-${neighbourMines}`;
        }
      }
    },
    countNeighbourMines: function(tile) {
      return this.neighbours(tile).filter((neighbour) => {
        return neighbour.mine;
      }).length;
    },
    neighbours: function(tile) {
      const theNeighbours = [];
      [[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1],
        [1, -1], [1, 0], [1, 1]].forEach((offset) => {
        let x = tile.row + offset[0];
        let y = tile.column + offset[1];
        if (this.valid(x, y)) {
          theNeighbours.push(this.tiles[x][y]);
        }
      });
      return theNeighbours;
    },
    valid: function(row, column) {
      return (row >= 0 && row < this.tiles.length) && (column >= 0 && column < this.tiles[0].length);
    },
    /**
     * opens all tiles
     * @function
     * @return {undefined}
     */
    allOpenTiles:function(){
      this.tiles.forEach((row) => {
        row.forEach((tile)=> {
          if(tile.mine){
            tile.class = 'mine';
          }
          else{
            if(tile.class === 'unopened'){
              tile.class = 'opened'; 
            }  
          }
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
      else {
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
