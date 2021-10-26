<template>
  <div id="app">
    <h3>To Do app</h3>
    <p>Available Task {{tasksLength}}</p>
    <ToDo msg="Welcome to Your Vue.js App" @showTasksCount="handleCustom"/>
  </div>
</template>

<script>
import ToDo from './components/ToDo.vue'

export default {
  name: 'App',
  components: {
    ToDo
  },
  data() {
    return {
      tasksLength: 0,
    }
  },
  methods: {
    handleCustom(tasks) {
      console.log("event...",tasks)
      this.tasksLength = tasks;
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
