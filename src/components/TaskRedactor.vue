<template>
  <form @submit="onSubmit" class="edit-form">
    <div class="form-control">
      <input
        type="text"
        v-model="task.text"
        name="text"
        placeholder="Task Name"
      />
    </div>

    <div class="form-controls-row">
      <div class="form-control">
        <input
          type="text"
          v-model="task.day"
          name="day"
          placeholder="Day & Time"
        />
      </div>
      <div class="form-control form-control-check">
        <label for="reminder">Reminder</label>
        <input type="checkbox" v-model="task.reminder" name="reminder" />
      </div>
    </div>

    <div class="form-controls-row">
      <div class="form-control form-control-select">
        <label for="reminder">Priority</label>
        <select v-model="task.priority" name="reminder">
          <option value="high">High</option>
          <option value="normal">Normal</option>
          <option value="low">Low</option>
        </select>
      </div>
    </div>

    <div class="form-controls-row">
      <input type="submit" value="Save Changes" class="btn btn-block" />
      <input
        type="button"
        value="Cancel"
        class="btn btn-block btn-cancel"
        @click="$emit('discard-changes')"
      />
    </div>
  </form>
</template>

<script>
export default {
  name: "TaskRedactor",
  props: {
    task: Object,
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      if (!this.task.text) {
        alert("Please add the task text");
        return;
      } else if (!this.task.day) {
        alert("Please add the task date");
        return;
      }

      this.$emit("change-task", this.task.id);
      this.$emit("hide-editor");
    },
  },
  emits: ["change-task", "hide-editor", "discard-changes"],
};
</script>

<style scoped>
.edit-form {
  padding: 10px;
  padding-top: 2px;
}

.form-control {
  margin-bottom: 10px;
  width: 100%;
}

.form-control-check {
  display: flex;
  align-items: center;
  font-size: 20px;
  width: 150px;
  margin-left: 20px;
}

.form-control-select {
  display: flex;
  align-items: center;
  font-size: 20px;
}

.form-control input {
  width: 100%;
  height: 40px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control input[type="checkbox"] {
  height: 30px;
  width: 30px;
  margin-left: 20px;
}

.form-control select {
  margin-left: 20px;
  padding: 5px 7px;
  font-size: 17px;
}

.form-controls-row {
  display: flex;
  justify-content: space-between;
}

.edit-form .btn {
  margin: 0;
}

.edit-form .btn-cancel {
  margin-left: 10px;
  background-color: rgb(255, 68, 0);
  width: 100px;
}
</style>
