<template>
  <div class="todo-app">
    <h1>Todo App</h1>
    <span v-if="editedTask !== null" class="editing">Editing...</span>
    <div class="inputs">
      <input type="text" placeholder="Enter Task" v-model="input" />
      <button v-if="editedTask === null" @click="submitTask()">Add Task</button>
      <button v-else @click="submitTask()">Edit Task</button>
    </div>
    <table>
      <tr>
        <th>Task</th>
        <th>Status</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
      <tr v-for="(task, i) in tasks" :key="i">
        <td>
          <span :class="{ completed: task.status === 'Completed' }">{{
            task.name
          }}</span>
        </td>
        <td
          class="pointer"
          :class="{
            'text-danger': task.status === 'To-Do',
            'text-warning': task.status === 'In-Progress',
            'text-success': task.status === 'Completed',
          }"
          style="width: 300px"
          @click="toggleStatus(i)"
        >
          <span>{{ task.status }}</span>
        </td>
        <td style="width: 250px">
          <div @click="editTask(i)"><span class="fa fa-pen"></span></div>
        </td>
        <td style="width: 250px">
          <div @click="deleteTask(i)"><span class="fa fa-trash"></span></div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      input: "",
      editedTask: null,
      statusStates: ["To-Do", "In-Progress", "Completed"],
      tasks: [
        {
          name: "Get Milk",
          status: "To-Do",
        },
        {
          name: "Get Bread",
          status: "In-Progress",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.input.length === 0) {
        alert("Please Enter a Task");
        return;
      }

      if (this.editedTask === null) {
        this.tasks.push({ name: this.input, status: "To-Do" });
        this.input = "";
      } else {
        this.tasks[this.editedTask].name = this.input;
        this.editedTask = null;
        this.input = "";
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.input = this.tasks[index].name;
      this.editedTask = index;
    },
    toggleStatus(index) {
      let newInd = this.statusStates.indexOf(this.tasks[index].status);
      newInd++;
      if (newInd > 2) {
        newInd = 0;
      }
      this.tasks[index].status = this.statusStates[newInd];
    },
  },
};
</script>
<style scoped>
.todo-app {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pointer {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
.text-danger {
  background: #ff4d4d;
}
.text-warning {
  background: #ff944d;
}
.text-success {
  background: #b33693;
}
* {
  font-family: "Poppins", sans-serif;
  color: white;
}

th {
  font-size: 20px;
  font-weight: 700;
  background: #c7417b;
}
td {
  font-size: 12px;
  font-weight: 300;
  text-align: center;
  font-size: 20px;
  border-radius: 10px;
  background: #8f3b76;
}

table {
  width: 80%;
}
tr {
  line-height: 50px;
}
.inputs {
  display: flex;
  justify-content: center;
}
input {
  height: 30px;
  margin-bottom: 30px;
  text-align: center;
  border-radius: 5px;
  font-weight: 600;
  font-size: 20px;
  color: black;
}
button {
  height: 36px;
  margin-left: 10px;
  font-weight: 600;
  background: #f5487f;
  border-radius: 5px;
  border-color: #f5487f;
}
.editing {
  font-size: 22px;
  font-weight: 500;
}
</style>
