<template>
    <div>
        <AddTask v-if="showAddTask" @add-task="addTask" />
        <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
    </div>
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'

export default {
    name: 'Home',
    components: {
        Tasks,
        AddTask
    },
    props: {
        showAddTask: Boolean,
    },
    data() {
        return {
            tasks: []
        }
    },
    methods: {
        async deleteTask(id) {
            if (confirm('Are you sure to delete this task ?')) {
                const res = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE'
                })

                res.status === 200 ? (this.tasks = this.tasks.filter(task => task.id !== id)) : alert(`Error: Delete Task ${id}`)
            }

        },
        async toggleReminder(id) {
            const taskToToggle = await this.fetchTask(id)
            const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }

            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                headers: { 'Content-type': 'application/json' },
                body: JSON.stringify(updTask)
            })

            const data = await res.json();
            this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder } : task)
        },
        async addTask(newTask) {
            const res = await fetch('api/tasks', {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify(newTask),
            })

            const data = await res.json()

            // this.tasks.push(newTask)
            this.tasks = [...this.tasks, data]
        },
        async fetchTasks() {
            const res = await fetch('api/tasks')
            const data = await res.json()

            return data
        },
        // fetch task with specific id
        async fetchTask(id) {
            const res = await fetch(`api/tasks/${id}`)
            const data = await res.json()

            return data
        }
    },

    // get data on API
    async created() {
        this.tasks = await this.fetchTasks()
    }

}
</script>