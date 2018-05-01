<template>
  <div class="maze-wrapper">
    <div id="maze" class="maze" :style="{width: brickSize * cols + 'px'}">

      <template v-for="(row, index_x) in rows">
        <template v-for="(col, index_y) in cols">

          <div class="arrow arrow-left" v-if="index_y==0" v-bind:style="{width: brickSize + 'px', height: brickSize + 'px', top: index_x * brickSize + 'px', left: -brickSize + 'px', lineHeight: brickSize + 'px'}" @click="pushLeft(index_x)">→{{index_x}}</div>
          <div class="arrow arrow-up" v-if = "index_x == 0" :style = "{width: brickSize + 'px', height: brickSize + 'px', top: -brickSize + 'px', left: index_y * brickSize + 'px', lineHeight: brickSize + 'px'}" @click = "pushUp(index_y)">↓{{index_y}}</div>

            <Brick :bricksize="brickSize" :xx="index_x" :yy="index_y" />

          <div class="arrow arrow-down" v-if="index_x==cols-1" v-bind:style="{width: brickSize + 'px', height: brickSize + 'px', top: rows * brickSize + 'px', left: index_y * brickSize + 'px', lineHeight: brickSize + 'px'}" @click="pushDown(index_y)">↑{{index_y}}</div>
          <div class="arrow arrow-right" v-if="index_y==cols-1" v-bind:style="{width: brickSize + 'px', height: brickSize + 'px', top: index_x * brickSize + 'px', left: cols * brickSize + 'px', lineHeight: brickSize + 'px'}" @click="pushRight(index_x)">←{{index_x}}</div>

        </template>
      </template>

    </div>
  </div>
</template>

<script>
import Brick from './Brick'

export default {
  name: 'Maze',
  data () {
    return {
      brickSize: 50,
      cols: 4,
      rows: 5
    }
  },
  components: {
    Brick
  },
  computed: {
    mazeArr: function () {
      //console.log('maze init');
      var arr = new Array(this.rows);
      for (var i = 0; i < this.rows; i++) {
        arr[i] = new Array(this.cols);
      }
      return arr
    }
  },
  methods: {
    test: function (xx, yy) {
      console.log(this.mazeArr);
      console.log(xx+' '+yy);
      this.mazeArr[0][0].x = 7;
    },
    pushLeft: function (row) {
      /*
      var sliceArr = [];
      this.mazeArr[row].forEach(function(el, index){
        // TODO: refactor maze array
        //var _el = el;
        var _el = Object.assign({}, el)
        //var _el = Object.create(el)
        //var _el = clone(el)
        //var _el = JSON.parse(JSON.stringify(el))

        sliceArr.push(_el);
        el.y += 1;
      });
      */

      var jumper = this.mazeArr[row][this.cols-1];
      jumper.y += 1;
      for (var i = this.cols - 1; i >= 0; --i) {
        if(this.mazeArr[row][i-1]){
          this.mazeArr[row][i] = this.mazeArr[row][i-1];
          this.mazeArr[row][i-1].y += 1;
        }
      };
      this.mazeArr[row][0] = jumper;
      jumper.y = 0;
    },
    pushRight: function (row) {
      var jumper = this.mazeArr[row][0];
      jumper.y -= 1;
      for (var i = 0; i < this.cols; i++) {
        if(this.mazeArr[row][i+1]){
          this.mazeArr[row][i] = this.mazeArr[row][i+1];
          this.mazeArr[row][i].y -= 1;
        }
      }
      this.mazeArr[row][this.cols-1] = jumper;
      jumper.y = this.cols-1;
    },
    pushUp: function (col) {
      var jumper = this.mazeArr[this.rows-1][col];
      jumper.x += 1;
      for (var i = this.rows - 1; i >= 0; --i) {
        if(this.mazeArr[i-1]){
          this.mazeArr[i][col] = this.mazeArr[i-1][col];
          this.mazeArr[i-1][col].x += 1;
        }
      };
      this.mazeArr[0][col] = jumper;
      jumper.x = 0;
    },
    pushDown: function (col) {
      var jumper = this.mazeArr[0][col];
      jumper.x -= 1;
      for (var i = 0; i < this.rows; i++) {//for (var i = this.rows - 1; i >= 0; --i) {
        if(this.mazeArr[i+1]){
          this.mazeArr[i][col] = this.mazeArr[i+1][col];
          this.mazeArr[i][col].x -= 1;
        }
      };
      this.mazeArr[this.rows-1][col] = jumper;
      jumper.x = this.rows-1;
    },
    addBrick: function (obj, x, y) {
      this.mazeArr[x][y]=obj;
      console.log(x+''+y);
    }
  },
  created() {
    this.$eventHub.$on('brickCreated', this.addBrick);
  },
  beforeDestroy() {
    this.$eventHub.$off('brickCreated');
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .maze-wrapper{
    display: inline-block;
    margin-top: 20px;
  }
  .maze{
    position:relative;
    /*display: grid;*/
    /*grid-template-columns: 20px 20px 20px 20px 20px;*/
  }
  .arrow{
    position:absolute;
    /*width:20px;*/
    /*height:20px;*/
    font-size: 13px;
    cursor:pointer;
  }
  /*.arrow-left{*/
    /*left:-25px;*/
  /*}*/
  /*.arrow-right{*/
    /*right:-25px;*/
  /*}*/
  /*.arrow-up{*/
    /*top:-25px;*/
  /*}*/
  /*.arrow-down{*/
    /*bottom:-25px;*/
  /*}*/
</style>
