<template>
  <div>
    <input
      v-on:keyup.enter="addTodo"
      placeholder="Add not todo"
      type="text"
      v-model="inputValue"
    />
    <button @click="addTodo">ADD</button>
  </div>
  <div class="single-todo" v-for="(todo, index) in filtered" :key="index">
    <div>
      <input
        :checked="todo.isDone"
        @click="changeIsDone(index)"
        type="checkbox"
        class="button-status"
      />
      <span :class="{ taskIsDone: todo.isDone }">{{ todo.name }}</span>
    </div>
    <button class="button-remove" @click="removeTodo(index)">x</button>
  </div>
  <div>
    <button @click="filter = 'all'">All</button>
    <button @click="filter = 'inProgress'">In progress</button>
    <button @click="filter = 'completed'">Completed</button>
  </div>
</template>
<style lang="scss">
@import "../styles/reset.scss";
@import "../styles/App.scss";
</style>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "HomeView",
  data: () => ({
    filter: "all",
    todos: [] as {
      name: string;
      isDone: boolean;
    }[],
    inputValue: "",
  }),
  computed: {
    filtered() {
      if (this.filter === "all") {
        return this.todos;
      } else if (this.filter === "inProgress") {
        return this.todos.filter((todo) => !todo.isDone);
      } else if (this.filter === "completed") {
        return this.todos.filter((todo) => todo.isDone);
      }
      return this.todos;
    },
  },
  methods: {
    addTodo() {
      console.log(this.todos);
      if (this.inputValue) {
        this.todos.push({
          name: this.inputValue,
          isDone: false,
        });
      }
      this.inputValue = "";
    },
    removeTodo(index: number) {
      this.todos.splice(index, 1);
    },
    changeIsDone(index: number) {
      if (this.todos[index]) {
        this.todos[index].isDone = !this.todos[index].isDone;
      }
    },
  },
});
</script>
