<template>
  <div class="container">
  <TaskProgress v-bind:todo="countSpecificTask('to-do')" v-bind:progress="countSpecificTask('in-progress')" v-bind:finished="countSpecificTask('finished')"></TaskProgress>
  <CreateTask @addTask="handleAddTask"></CreateTask>
  <TaskList v-bind:tasks="tasks"></TaskList>
    
  </div>
</template>

<script>
import TaskProgress from './TaskProgress.vue';
import CreateTask from './CreateTask.vue';
import TaskList from './TaskList.vue';
export default {
  name: 'ToDo',
  props: {
    msg: String
  },
  components: {
    TaskProgress, CreateTask, TaskList
  },
  mounted() {
    this.countTask();
  },
  data() {
    return {
      tasks: [ 
        {task: "task 1", status: "to-do", edit: false},
        {task: "task 2", status: "to-do", edit: false},
        {task: "task 3", status: "in-progress", edit: false},
        {task: "task 4", status: "to-do", edit: false},
        {task: "task 5", status: "finished", edit: false},
      ],
    }
  },

  methods : {
    
    countTask() {
      this.$emit('showTasksCount', this.tasks.length);
    },
    countSpecificTask(task) {
      return this.tasks.reduce((previousValue, currentObject) => {
          return previousValue + (currentObject.status === task); 
      }, 0);
    },

    handleAddTask(task) {
      this.tasks.push(task)
      this.countTask()
    }
  }
}
</script>
