<template>
  <div id="app">
    <h4 class="col-8 bg-primary text-white text-center p-2">{{name}}'s To Do List</h4>
    <div class="container-fluid p-4">
      <div class="row" v-if="filteredTasks.length == 0">
        <div class="col text-center">
          <b>Nothing to do. Hurrah!</b>
        </div>
      </div>
      <template v-else>
        <div class="row col-8">
          <div class="col font-weight-bold">Task</div>
          <div class="col-2 font-weight-bold">When</div>
          <div class="col-2 font-weight-bold">Done</div>
        </div>
        <div class="row col-8" v-for="t in filteredTasks" v-bind:key="t.action">
          <div class="col">{{t.action}}</div>
          <div class="col-2">{{t.when}}</div>
          <div class="col-2 text-center">
            <input type="checkbox" v-model="t.done" class="form-check-input">
          </div>
        </div>
      </template>
      <div class="row col-8 py-2">
        <div class="col">
          <input v-model="newItemText" class="form-control" placeholder="Enter a reminder">
        </div>
        <div class="col-2">
          <input v-model="whenItemText" class="form-control" placeholder="When">
        </div>
        <div class="col-2">
          <button class="btn btn-primary" v-on:click="addNewTodo">Add</button>
        </div>
      </div>

      <div class="row col-8 bg-secondary py-2 mt-2 text-white">
        <div class="col text-center">
          <input type="checkbox" v-model="hideCompleted" class="form-check-input">
          <label class="form-check-label font-weight-bold">Hide completed tasks</label>
        </div>
        <div class="col-2 text-center">
          <button class="btn btn-sm btn-warning" v-on:click="deleteCompleted">Delete Done</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      name: "Riddick",
      tasks: [
        // { action: "Kiss the girl", when: "5am", done: false },
        // { action: "Fetch water from well", when: "5am", done: false },
        // { action: "Eat a pig", when: "9am", done: true },
        // { action: "Kick a rugby team's ass", when: "11am", done: false }
      ],
      hideCompleted: true,
      newItemText: "",
      whenItemText: ""
    };
  },
  computed: {
    filteredTasks() {
      {
        return this.hideCompleted
          ? this.tasks.filter(t => !t.done)
          : this.tasks;
      }
    }
  },
  methods: {
    addNewTodo() {
      this.tasks.push({
        action: this.newItemText,
        when: this.whenItemText,
        done: false
      });
      this.storeData();
      this.newItemText = "";
      this.whenItemText = "";
    },
    storeData() {
      localStorage.setItem("actions", JSON.stringify(this.tasks));
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter(t => !t.done);
      this.storeData();
    }
  },
  created() {
    let data = localStorage.getItem("actions");
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  }
};
</script>

<style></style>
