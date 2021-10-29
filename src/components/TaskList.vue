<template>
	<div>
		<table class="table table-bordered mt-3">
			<thead>
				<tr>
					<TableHeader v-bind:label="'ID'" v-bind:width="100"></TableHeader>
					<TableHeader v-bind:label="'Task'"></TableHeader>
					<TableHeader v-bind:label="'Status'" v-bind:width="180"></TableHeader>
					<TableHeader v-bind:label="'Action'" v-bind:width="180"></TableHeader>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(task, index) in tasks" :key="index">
					<TableData v-bind:rowIndex="index" v-bind:column="'Index'" v-bind:taskItem="task"></TableData>
					<TableData v-bind:rowIndex="index" v-bind:column="'Edit'" v-bind:taskItem="task" @updateTask="handleUpdateTask"></TableData>
					<TableData v-bind:rowIndex="index" v-bind:column="'Status'" v-bind:taskItem="task" @updateStatus="handleUpdateStatus"></TableData>
					<TableData v-bind:rowIndex="index" v-bind:column="'Actions'" v-bind:taskItem="task" @removeTask="handleRemoveTask"></TableData>

				</tr>
			</tbody>
		</table>

	</div>
</template>

<script>
import TableHeader from './TableHeader.vue'
import TableData from './TableData.vue'
export default {
	name: 'TaskList',
	props: {tasks: Array},
	components: {TableHeader,TableData},
	mounted() {
	},
	data() {
		return {
			availabelStatus: ['to-do', 'in-progress','finished'],
		}
	},
	methods: {
		handleRemoveTask(index) {
			this.tasks.splice(index,1)
		},
		handleUpdateTask(index, data) {
			console.log("updates....",index, ' ',data)
			this.tasks[index].task = data;
		},
		handleUpdateStatus(index) {
      const indexObject = {"index": 0}
      indexObject.index = this.availabelStatus.indexOf(this.tasks[index].status) + 1;
      this.tasks[index].status = this.availabelStatus[indexObject.index > 2 ? indexObject.index = 0 : indexObject.index];
			this.tasks.map((item) => item.edit = false)
    },
	}
}
</script>

