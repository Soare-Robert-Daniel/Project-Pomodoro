<template>
  <div id="app">

    <b-container class="bv-example-row">
      <b-row class="justify-content-md-center">
        <b-col></b-col>
        <b-col md="auto">

          <b-jumbotron bg-variant="info" text-variant="white" border-variant="dark" header="Pomodoro Timer">

          </b-jumbotron>
          <Timer />

        </b-col>
        <b-col></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Timer from './components/Timer.vue'

export default {
  name: 'app',
  components: {
    Timer
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
