<template>
    <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input 
        type="text" 
        v-model="text"
        name="text" 
        placeholder="Add Task" />
    </div>

    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        v-model="day"
        name="day"
        placeholder="Add Day & Time"
      />
    </div>

    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input 
        type="checkbox" 
        v-model="reminder"
        name="reminder" />
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
export default {
    name: 'Add Task',
    data() {
        return {
            // form default value
            text: '',
            day: '',
            reminder: false,
        }
    },
    methods: {
        onSubmit(e) {
            e.preventDefault();
            
            // vaidation
            if(!this.text){
                alert('Please add a task')
                return
            }

            const newTask = {
                // id: Math.floor(Math.random() * 12000),
                text: this.text,
                day: this.day,
                reminder: this.reminder
            }

            // emit upward
            this.$emit('add-task', newTask)

            // reset input fields after submiting - clear form
            this.text = ''
            this.day = ''
            this.reminder = false
        }
    }
}
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>