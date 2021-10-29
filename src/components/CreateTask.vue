<template>
	<div class="d-flex mt-3">
		<input v-model="task" type="text" placeholder="Enter task name" class="form-control" v-on:keyup.enter="submitNewTask"/>
		<button class="btn btn-primary rounded-0" v-on:click="submitNewTask">Submit</button>
	</div>
</template>

<script>
export default {
  name: 'CreateTask',
  props: {},
	data() {
		return {
			task: '',
			taskObject: {task: String, status: String, edit: Boolean}
		}
	},
  methods : {
    
    submitNewTask() {
			if(this.task === '') return;
			this.taskObject = {}
      this.taskObject.task= this.task; this.taskObject.status= "to-do"; this.taskObject.edit= false;
			
			this.$emit('addTask',JSON.parse(JSON.stringify(this.taskObject)));
      this.task = '';
    },
  }
}
</script>