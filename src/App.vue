<template>
  <div>
    <h1 class="title">Click the button and I'll tell you a joke!</h1>
    <Jokes />
  </div>
</template>

<script>
import Jokes from './components/Jokes.vue'

export default {
  name: 'App',
  components: {
    Jokes
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    width: 100vw;
    height: 100vh;
    background-image: linear-gradient(to left bottom, #5BFB45, #7DC673);
    background-repeat: no-repeat;
    font-family: sans-serif;
  }
  .title {
    font-size: 36px;
    color: #1E241D;
    text-align: center;
    margin-top: 5%;

  }
</style>
