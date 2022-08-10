<template>
  <header>
    <h1>{{ heading }}</h1>
    <Button text="Add Task" color="green" />
  </header>
</template>

<script>
import Button from "./Button.vue";

export default {
  name: "Header",
  props: {
    heading: String,
  },
  components: { Button },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
