<template>
  <div id="app">
    <student></student>
  </div>
</template>

<script>
import Student from './view/Student.vue'

export default {
  name: 'App',
  components: {
    Student
  }
}
</script>

<style>
#app{
  padding: 20px 40px;
}
</style>
