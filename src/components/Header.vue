<template>
    <header>
        <h1>{{ title }}</h1>
        <Button 
          v-show="homePage" 
          @btn-click="$emit('toggle-add-task')"
          :text="showAddTask ? 'Close' : 'Add Task'" 
          :color="showAddTask ? 'Red' : 'Green'"/>
        
    </header>
</template>

<script>
import Button from './Button.vue'

export default {
  name: 'Task Tracker 2023',
  props: {
    title: String,
    showAddTask: Boolean

  },
  components: {
    Button
  },
  computed: {
    homePage(){
      if(this.$route.path === '/') return true
      else return false
    }
  },
  emits: ['toggle-add-task']

}
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>