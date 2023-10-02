<template>
	<div id="app">
		<h1>To do list</h1>
		<tasks-progress-vue :progress="progress"/>
		<new-task @taskAdded="addTask"/>
		<task-grid :tasks="tasks"
			@taskDeleted="deleteTask"
			@taskStateChanged="toggleTaskState" /> 
	</div>
</template>

<script>
import TaskGrid from './components/TaskGrid.vue';
import NewTask from './components/NewTask.vue';
import TasksProgressVue from './components/TasksProgress.vue';

export default {
	components: {
		TaskGrid,
		NewTask,
		TasksProgressVue
	},

	data() {
		return {
			tasks: []
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			reallyNew && this.tasks.push({
				name: task.name,
				pending: task.pending || true
			})
		},
		deleteTask(task)  {
			const i  = this.tasks.indexOf(task)
			if(i >= 0) this.tasks.splice(i, 1)
		},
		toggleTaskState(task) {
			const i  = this.tasks.indexOf(task)
			this.tasks[i].pending = !this.tasks[i].pending
		}
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
