<template>
  <q-page class="col">
    <div class="row-12">
      <timer></timer>
    </div>
    <div class="row-12">
      <todo-list></todo-list>
    </div>
  </q-page>
</template>

<script>
import Timer from 'components/Timer'
import TodoList from 'components/TodoList'
export default {
  name: 'PageIndex',
  components: {
    Timer,
    TodoList
  }
}
</script>
<style lang="sass">
.q-page
  background: url("https://i.imgur.com/inQZ4dJ.jpg") no-repeat 50% fixed

</style>

