<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input type="text" v-model="text" name="text" placeholder="Add Task" />
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
    <div class="form-control form-control-row">
      <div class="input-group">
        <label>Set Reminder</label>
        <input type="checkbox" v-model="reminder" name="reminder" />
      </div>

      <div class="input-group">
        <label>Choose Priority</label>
        <select v-model="priority" name="priority">
          <option value="high">High</option>
          <option value="normal">Normal</option>
          <option value="low">Low</option>
        </select>
      </div>
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: "AddTask",
  data() {
    return {
      text: "",
      day: "",
      reminder: "",
      priority: "normal",
    };
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      if (!this.text) {
        alert("Please add the task text");
        return;
      } else if (!this.day) {
        alert("Please add the task date");
        return;
      }

      const newTask = {
        text: this.text,
        day: this.day,
        reminder: this.reminder,
        priority: this.priority,
      };

      this.text = "";
      this.day = "";
      this.reminder = false;
      this.priority = "normal";

      this.$emit("add-task", newTask);
    },
  },
};
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

.form-control-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.input-group {
  display: flex;
  align-items: center;
}

.input-group input {
  height: 30px;
  width: 30px;
  margin: 0;
  margin-left: 20px;
}

.input-group select {
  margin-left: 20px;
  padding: 5px 7px;
  font-size: 17px;
}
</style>
