<template>
  <div
    :class="[task.reminder ? 'reminder' : '', task.priority, 'task']"
    @dblclick="$emit('toggle-reminder', task.id)"
  >
    <h3>
      {{ task.text }}
      <div class="controls">
        <i class="fas fa-bell" @click="$emit('toggle-reminder', task.id)"></i>
        <i class="far fa-edit" @click="$emit('toggle-editor', task)"></i>
        <i class="fas fa-times" @click="$emit('delete-task', task.id)"></i>
      </div>
    </h3>

    <p>
      {{ task.day }}
    </p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
};
</script>

<style scoped>
.controls {
  display: flex;
  margin-left: auto;
}

.fa-times {
  color: red;
  font-size: 29px;
}

.fa-edit {
  color: rgb(93, 0, 255);
  font-size: 26px;
  margin-right: 12px;
}

.fa-bell {
  display: none;
  font-size: 29px;
  margin-right: 16px;
  color: #444;
}

.reminder .fa-bell {
  display: block;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  padding-right: 12px;
  cursor: pointer;
}

.task.low {
  background-color: #ecf4ec;
  border-left: 5px solid green;
}

.task.normal {
  background-color: #f4f4e4;
  border-left: 5px solid #ffae42;
}

.task.high {
  background-color: #fff2f2;
  border-left: 5px solid red;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
