<template>
  <header>
    <h1>
      {{ title }}
    </h1>
    <Button
      :text="showAddTask ? 'Close' : 'Add task'"
      :color="showAddTask ? 'red' : 'green'"
      v-show="homePage"
      @btn-click="$emit('toggle-add-task')"
    />
  </header>
</template>

<script>
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: String,
    showAddTask: Boolean,
  },
  components: {
    Button,
  },
  computed: {
    homePage() {
      return this.$route.path == "/";
    },
  },
  emits: ["toggle-add-task"],
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
