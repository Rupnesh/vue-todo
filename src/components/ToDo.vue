<template>
  <div class="container">
  <div class="d-flex justify-content-center">
    
    <span class="btn btn-light">ToDo {{countSpecificTask('to-do')}}</span>
    <span class="btn btn-light">InProgress {{countSpecificTask('in-progress')}}</span>
    <span class="btn btn-light">Finished {{countSpecificTask('finished')}}</span>
    <button class="btn btn-light" @click="countTask()"><span class="fa fa-sync"></span></button>
  </div>
    

    <div class="d-flex mt-5">
      <input v-model="task" type="text" placeholder="Enter task name" class="form-control" v-on:keyup.enter="onEnter"/>
      <button class="btn btn-primary rounded-0" @click="submitTask">Submit</button>
    </div>

    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col" style="width: 100px">ID</th>
          <th scope="col">Task</th>
          <th scope="col" style="width: 180px">Status</th>
          <th scope="col" style="width: 180px">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th scope="row" style="width: 100px">{{index+1}}</th>
          <td><span :class="{'finished': task.status ==='finished'}">{{task.task}}</span></td>
          <td style="width: 180px" class="text-capitalize" role="button"><span @click="changeStatus(index)" :class="{'text-danger': task.status ==='to-do', 'text-warning': task.status ==='in-progress','text-success': task.status ==='finished'}">{{task.status}}</span></td>
          <td class="d-flex" style="width: 180px">
            <div style="flex:1" @click="editTask(index)" role="button"><span class="fa fa-pen"></span></div>
            <div style="flex:1" class="text-danger" @click="deleteTask(index)" role="button"><span class="fa fa-trash"></span></div>
          </td>
        </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      task: '',
      editedTask: null,
      availabelStatus: ['to-do', 'in-progress','finished'],
      tasks: [ 
        {task: "task 1", status: "to-do"},
        {task: "task 2", status: "to-do"},
        {task: "task 3", status: "in-progress"},
        {task: "task 4", status: "to-do"},
        {task: "task 5", status: "finished"},
      ]
    }
  },

  methods : {
    submitTask() {
      if(this.task.length === 0 ) return;
      if(this.editedTask === null)
        this.tasks.push({task: this.task, status: "to-do"})
      else {
        this.tasks[this.editedTask].task = this.task;
        this.tasks[this.editedTask].status = this.availabelStatus[0];
        this.editedTask = null;
      }
      this.task = ''
    },
    deleteTask(index) {
      this.tasks.splice(index,1)
    },
    editTask(index) {
      this.task = this.tasks[index].task;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availabelStatus.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availabelStatus[newIndex];
    },
    onEnter() {
      this.submitTask();
    },
    countTask() {
      this.$emit('showTasksCount', this.tasks.length);
    },
    countSpecificTask(task) {
      var result = this.tasks.reduce((previousValue, currentObject) => {
          return previousValue + (currentObject.status === task); 
      }, 0);
      return result;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .finished {
    text-decoration : line-through;
  }
</style>
