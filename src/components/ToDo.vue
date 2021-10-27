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
          <th scope="col">Task<span class="fa fa-pen" style="margin-left: 12px"></span></th>
          <th scope="col" style="width: 180px">Status</th>
          <th scope="col" style="width: 180px">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th scope="row" style="width: 100px">{{index+1}}</th>
          <td>
            <span  v-show = "task.edit === false" :class="{'finished': task.status ==='finished'}" @dblclick="task.edit = true" >{{task.task}}</span>
            <!--<input v-else v-model="tasks[index].task" type="text" placeholder="Enter task name" class="form-control" v-on:keyup.enter="onEnter"/>
          -->
          <input v-show="task.edit == true" v-model = "task.task" v-on:blur= "task.edit=false;" 
              @keyup.enter="task.edit=false; update(index,task)">
          
          </td>
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
  mounted() {
    this.countTask();
  },
  data() {
    return {
      task: '',
      editedTask: null,
      editTaskText: false,
      availabelStatus: ['to-do', 'in-progress','finished'],
      tasks: [ 
        {task: "task 1", status: "to-do", edit: false},
        {task: "task 2", status: "to-do", edit: false},
        {task: "task 3", status: "in-progress", edit: false},
        {task: "task 4", status: "to-do", edit: false},
        {task: "task 5", status: "finished", edit: false},
      ]
    }
  },

  methods : {
    
    submitTask() {
      if(this.task.length === 0 ) return;
      if(this.editedTask === null)
        this.tasks.push({task: this.task, status: "to-do", edit: false})
      else {
        this.tasks[this.editedTask].task = this.task;
        this.tasks[this.editedTask].status = this.availabelStatus[0];
        this.editedTask = null;
      }
      this.task = '';
      this.countTask();
    },
    deleteTask(index) {
      this.$dialog
      .confirm('Please confirm to continue')
      .then(dialog=> {
        console.log('Clicked on proceed',dialog);
        this.tasks.splice(index,1)
      })
      .catch(()=> {
        console.log('Clicked on cancel');
      });
      
    },
    editTask(index) {
      this.task = this.tasks[index].task;
      this.editedTask = index;
      this.editTaskText = true;
    },
    changeStatus(index) {
      const newIndex = this.availabelStatus.indexOf(this.tasks[index].status) + 1;
      this.tasks[index].status = this.availabelStatus[newIndex > 2 ? newIndex = 0 : newIndex];
    },
    onEnter() {
      this.submitTask();
    },
    countTask() {
      this.$emit('showTasksCount', this.tasks.length);
    },
    countSpecificTask(task) {
      return this.tasks.reduce((previousValue, currentObject) => {
          return previousValue + (currentObject.status === task); 
      }, 0);
      
    },
    update(index,data) {
      this.tasks[index].task = data.task;
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
