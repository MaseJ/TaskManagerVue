<template>
    <div v-if="showForm">
		<AddTask @add-task="addTask" />
	</div>
	<Tasks @delete-task="deleteTask" :tasks="tasks" 
		@toggle-reminder="toggleReminder"
	/>
</template>

<script>
    import Tasks from "../components/Tasks.vue"
	import AddTask from "../components/AddTask.vue"

    export default {
        name: 'Home',
        props: {
            showForm: Boolean
        },
        components: {
            Tasks,
            AddTask
        },
        data() {
            return {
                tasks: []
            }
            
        },
        methods: {
            async fetchTasks() {
				const res = await fetch('api/tasks')
				
				const data = await res.json()

				return data
			},
			async fetchTask(id) {
				const res = await fetch(`api/tasks/${id}`)
				
				const data = await res.json()

				return data
			},
            async deleteTask(id){
				const res = await fetch(`api/tasks/${id}`, {
					method: 'DELETE'
				})

				res.status === 200 ? ( this.tasks = this.tasks.filter(task => task.id !== id)) : alert("Error deleting task")

			},
			async toggleReminder(id){
				const taskToToggle = await this.fetchTask(id)
				const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

				const res = await fetch(`api/tasks/${id}`, {
					method: 'PUT',
					headers: {
						'Content-type': 'application/json'
					},
					body: JSON.stringify(updTask)
				})

				const data = await res.json()

				this.tasks = this.tasks.map(task => task.id === id ? {...task, reminder: data.reminder} : task)
			},
			async addTask(task){
				const res = await fetch('api/tasks', {
					method: 'POST',
					headers: {
						'Content-type': 'application/json'
					},
					body: JSON.stringify(task)
				})

				const data = await res.json()

				this.tasks = [data, ...this.tasks]
			}
        },
		async created() {
			this.tasks = await this.fetchTasks()
		}
    }
</script>