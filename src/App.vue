<template>
  <div class="app">
    <h2 class="app-title">Today's tasks</h2>
    <div class="wrapper-stat">
      <div class="stat busines">
        <h5>Business</h5>
        <div class="wrapper-pogress">
          <div
            class="bar business"
            :style="{ width: statistics.business }"
          ></div>
        </div>
      </div>
      <div class="stat personal">
        <h5>Personal</h5>
        <div class="wrapper-pogress">
          <div
            class="bar personal"
            :style="{ width: statistics.personal }"
          ></div>
        </div>
      </div>
    </div>
    <form class="task-add">
      <div class="task-add-inputs">
        <input
          class="task-add-input"
          type="text"
          placeholder="Add a task"
          v-model="taskName"
        />

        <div class="radio-buttons">
          <label class="form-control business">
            <input
              type="radio"
              name="radio"
              checked
              value="business"
              v-model="picked"
            />
            Business
          </label>

          <label class="form-control">
            <input
              type="radio"
              name="radio"
              value="personal"
              v-model="picked"
            />
            Personal
          </label>
        </div>
      </div>
      <button
        class="task-add-btn"
        @click="onAddTask"
        v-on:keyup.enter="onAddTask"
      >
        +
      </button>
    </form>
    <div class="tasks">
      <div
        class="task"
        v-for="task in tasks"
        :key="task.id"
        @click="onCompleteHandler(task._id)"
      >
        <div class="round" :class="task.type">
          <input type="checkbox" v-bind:id="task._id" />
          <label v-bind:for="task._id"></label>
        </div>
        <div class="task-item">
          <span :class="[task.isCompleted ? 'completed' : '']">{{
            task.name
          }}</span>
          <button class="remove-task" @click="onTaskDelete(task._id)">
            &#128465;
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

/**
  [x] - Add task when enter key
  [x] - Clear input when added task
  [x] - Check new task is emty or not
  [] - Choise color in input
  [x] - Proggres bar business & personal tasks
  [x] - Delete tasks
 */

<script>
export default {
  name: "App",
  data() {
    return {
      taskName: "",
      picked: "",
      tasks: [
        {
          _id: "1",
          name: "Crea todo app in vue js",
          isCompleted: false,
          type: "business",
        },
      ],
    };
  },
  methods: {
    onCompleteHandler: function (id) {
      const currentTask = this.tasks.find((t) => t._id === id);
      currentTask.isCompleted = !currentTask.isCompleted;
      console.log(currentTask.isCompleted);
    },
    onAddTask: function (e) {
      e.preventDefault();
      if (this.taskName === "") return;
      this.tasks.push({
        _id: Math.random().toString(36).substring(2, 7),
        name: this.taskName,
        isCompleted: false,
        type: this.picked,
      });
      this.taskName = "";
      console.log(this.taskName);
    },
    onTaskDelete: function (id) {
      this.tasks = this.tasks.filter((task) => task._id !== id);
    },
  },
  computed: {
    statistics: function () {
      const count = this.tasks.length || 1;
      const businessData = this.tasks.filter((t) => t.type === "business");
      const personalData = this.tasks.filter((t) => t.type === "personal");
      console.log(count, businessData, personalData);
      return {
        business: Math.round((businessData.length * 100) / count) + "%",
        personal: Math.round((personalData.length * 100) / count) + "%",
      };
    },
  },
};
</script>


