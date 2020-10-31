<template>
  <Title /> 
  <AddTodo />
</template>

<script>
  import Title from './components/Title'
  import AddTodo from './components/AddTodo'

  export default {
    name: 'app',
    components: {
      Title,
      AddTodo
    }
  }
</script>

<style lang="scss">
  * {
      font-family: Arial, Helvetica, sans-serif;
  }
</style>
