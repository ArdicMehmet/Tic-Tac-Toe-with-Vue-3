<template>
  <Tictactoe></Tictactoe>
</template>

<script>
import Tictactoe from "./components/Tictactoe.vue";

export default {
  name: 'App',
  data(){
    return{
    }
  },
  methods:{
  },
  components: {
    Tictactoe,
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  color: white;
  font-weight: 700;
  background-color: #2c3e50;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
