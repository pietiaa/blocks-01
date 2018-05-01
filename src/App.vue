<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <Maze/>
  </div>
</template>

<script>
import Maze from './components/Maze'
//import Brick from './components/Brick'

export default {
  name: 'App',
  components: {
    Maze
  },
  data () {
    return {
      title: 'The Labyrinth	Maze'
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    /*-webkit-font-smoothing: antialiased;*/
    /*-moz-osx-font-smoothing: grayscale;*/
    text-align: center;
    color: #2c3e50;
    /*margin-top: 60px;*/
  }
  html{
    background-color: mediumseagreen;
  }

</style>
