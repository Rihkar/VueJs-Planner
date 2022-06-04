<template>
  <div class="input-wrapper">
    <input
      v-on:keyup.enter="addTodo"
      placeholder="Add not todo"
      type="text"
      v-model="inputValue"
    />
    <button class="button-add" @click="addTodo">ADD</button>
  </div>
  <div class="single-todo-box-wrapper" v-for="todo in filtered" :key="todo.id">
    <div class="single-todo-box">
      <label class="checkbox-container">
        <input
          @click="changeIsDone(todo.id)"
          :checked="todo.isDone"
          :unchecked="!todo.isDone"
          type="checkbox"
          class="button-status"
        />
        <span class="checkmark"></span>
      </label>
      <span class="single-todo" :class="{ taskIsDone: todo.isDone }">
        {{ todo.name }}
      </span>
    </div>
    <button class="button-remove" @click="removeTodo(todo.id)">x</button>
  </div>
  <div class="buttons-filter-box">
    <button
      :class="{ selectedFilterButton: filterType === 'all' }"
      @click="filterType = 'all'"
    >
      All
    </button>
    <button
      :class="{ selectedFilterButton: filterType === 'inProgress' }"
      @click="filterType = 'inProgress'"
    >
      In progress
    </button>
    <button
      :class="{ selectedFilterButton: filterType === 'completed' }"
      @click="filterType = 'completed'"
    >
      Completed
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "HomeView",
  data: () => ({
    filterType: "all",
    todos: [] as {
      name: string;
      isDone: boolean;
      id: number;
    }[],
    inputValue: "",
    initialId: 0,
  }),
  computed: {
    filtered() {
      if (this.filterType === "all") {
        return this.todos;
      } else if (this.filterType === "inProgress") {
        return this.todos.filter((todo) => !todo.isDone);
      } else if (this.filterType === "completed") {
        return this.todos.filter((todo) => todo.isDone);
      }
      return this.todos;
    },
  },
  methods: {
    addTodo() {
      if (this.inputValue) {
        this.todos.push({
          name: this.inputValue,
          isDone: false,
          id: this.initialId++,
        });
      }
      this.inputValue = "";
    },
    removeTodo(index: number) {
      this.todos = this.todos.filter((el) => el.id !== index);
    },
    changeIsDone(todoId: number) {
      // const index = this.todos.findIndex((todo) => {
      //   return todoId === todo.id;
      // });
      // this.todos[index].isDone = !this.todos[index].isDone;
      this.todos.forEach((todo) => {
        if (todo.id === todoId) {
          todo.isDone = !todo.isDone;
        }
      });
    },
  },
});
</script>
