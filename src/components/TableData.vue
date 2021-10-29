<template>
  <td v-if="column == 'Index'">{{rowIndex+1}}</td>

  <td v-else-if="column == 'Edit'">
    <span v-show="taskItem.edit === false" :class="{'finished': taskItem.status ==='finished'}" >{{taskItem.task}}</span>
    <input v-show="taskItem.edit == true" v-model = "taskItem.task" v-on:blur= "taskItem.edit=false;" 
      @keyup.enter="taskItem.edit=false; update(rowIndex,taskItem)">
  </td>

  <td v-else-if="column == 'Status'" class="text-capitalize" role="button" @click="changeStatus(rowIndex)"><span :class="{'text-danger': taskItem.status ==='to-do', 'text-warning': taskItem.status ==='in-progress','text-success': taskItem.status ==='finished'}">{{taskItem.status}}</span></td>

  <td v-else-if="column == 'Actions'" class="d-flex">
    <div style="flex:1"  >
      <button v-bind:disabled="taskItem.status =='finished'" class="btn btn-light p-0" @click="editTask(rowIndex)"><span class="fa fa-pen"></span></button>
    </div>
    <div style="flex:1">
      <button v-bind:disabled="taskItem.status =='finished'" class="btn btn-light text-danger p-0" @click="deleteTask(rowIndex)"><span class="fa fa-trash"></span></button>
    </div>
  </td>

</template>

<script>
export default {
  name: 'TableData',
  props: {column: String, taskItem: Object, rowIndex: Number},
  mounted() {
  },
  methods: {
    editTask() {
      this.taskItem.edit = true;
    },
    deleteTask(index) {
      this.$dialog
      .confirm('Do you want to remove this task?')
      .then(dialog=> {
        console.log(dialog, index)
        this.$emit('removeTask',index);
      })
      .catch(()=> {
        console.log('Clicked on cancel');
      });
      
    },
    update(index,data) {
      this.$emit('updateTask',index, data.task);
    },
    changeStatus(index) {
      this.$emit('updateStatus',index);
    },
  }
}
</script>

<style scoped>
  .finished {
    text-decoration : line-through;
  }
</style>