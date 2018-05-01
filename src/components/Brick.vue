<template>
  <div class="brick" :style="[styleObj, stylePosObj]" :id="'b'+[x]+[y]"><span>{{x}}{{y}}</span></div>
</template>

<script>
export default {
  name: 'Brick',
  data () {
    return {
      x: this.xx,
      y: this.yy,
      bs: this.bricksize
    }
  },
  computed: {
    styleObj: function () {
      var colors = ['red','blue']
      var color = colors[Math.floor(Math.random()*colors.length)]
      return {
        backgroundColor: color,
        width: this.bs + 'px',
        height: this.bs + 'px',
        lineHeight: this.bs + 'px'
      }
    },
    stylePosObj: function () {
      return {
        left: this.y * this.bs + 'px',
        top: this.x * this.bs + 'px',
      }
    }
  },
  props: ['xx','yy','bricksize'],
  mounted() {
    this.$eventHub.$emit('brickCreated',this, this.xx, this.yy);
      //console.log(this.x+''+this.y)
//    this.$nextTick(function () {
//      this.$emit('dupa', 'ass')
//    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  div{
    position: absolute;
    box-sizing: border-box;
    background-color:green;
    /*border:1px solid yellow;*/
    transition: .5s;
  }

</style>
