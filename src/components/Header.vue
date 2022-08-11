<template>
  <header>
    <h1>{{ heading }}</h1>
    <Button
      :text="shouldShowAddTask ? 'Close' : 'Add Task'"
      :color="shouldShowAddTask ? 'red' : 'green'"
      @btn-click="$emit('toggle-add-task')"
    />
  </header>
</template>

<script>
import Button from "./Button.vue"

export default {
  name: "Header",
  props: {
    heading: String,
    shouldShowAddTask: Boolean,
  },
  components: { Button },
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
