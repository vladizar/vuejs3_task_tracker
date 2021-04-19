<template>
  <div :key="task.id" v-for="task in tasks">
    <Task
      :task="task"
      @delete-task="$emit('delete-task', task.id)"
      @toggle-reminder="$emit('toggle-reminder', task.id)"
      @toggle-editor="toggleTaskEditor"
    />
    <TaskRedactor
      v-show="showTaskEditorID == task.id"
      :task="task"
      @change-task="$emit('change-task', task.id)"
      @hide-editor="hideTaskEditor"
      @discard-changes="$emit('discard-changes', this.savedTask)"
    />
  </div>
</template>

<script>
import Task from "./Task";
import TaskRedactor from "./TaskRedactor";

export default {
  name: "Tasks",
  props: {
    tasks: Array,
  },
  components: {
    Task,
    TaskRedactor,
  },
  data() {
    return {
      showTaskEditorID: -1,
      savedTask: Object,
    };
  },
  methods: {
    toggleTaskEditor(task) {
      if (this.showTaskEditorID != -1) {
        if (JSON.stringify(task) != JSON.stringify(this.savedTask)) {
          if (confirm("Changes you have made won't be saved. Are you sure?")) {
            this.$emit("discard-changes", this.savedTask);

            this.showTaskEditorID =
              task.id == this.showTaskEditorID ? -1 : task.id;
          }
        } else {
          this.showTaskEditorID =
            task.id == this.showTaskEditorID ? -1 : task.id;
        }
      } else {
        this.showTaskEditorID = task.id;
        this.savedTask = { ...task };
      }
    },
    hideTaskEditor() {
      this.showTaskEditorID = -1;
    },
  },
  emits: ["delete-task", "toggle-reminder", "discard-changes", "change-task"],
};
</script>
