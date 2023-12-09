<script>
import TodosComponent from "./components/TodosComponent.vue";

export default {
  name: "App",
  components: {
    TodosComponent,
    // CompletedTasksComponent,
  },
  data() {
    return {
      todosData: [
        {
          userId: 1,
          id: 1,
          title: "Set a meeting time with Doctor",
          completed: true,
        },
        {
          userId: 1,
          id: 2,
          title: "Get bank statement for last time",
          completed: false,
        },
        {
          userId: 1,
          id: 3,
          title: "Read my Bible",
          completed: false,
        },
        {
          userId: 1,
          id: 4,
          title: "Overall checkup for weekend",
          completed: true,
        },
        {
          userId: 1,
          id: 5,
          title: "Text tailor for my Sunday dress",
          completed: false,
        },

        {
          userId: 1,
          id: 8,
          title: "Send Yimika's pictures after church",
          completed: true,
        },
        {
          userId: 1,
          id: 9,
          title: "Prepare for the new week",
          completed: false,
        },
      ],
    };
  },
  methods: {
    addTodo(todo) {
      this.todosData.unshift(todo);
    },
    deleteTodo(id) {
      const index = this.todosData.findIndex((todo) => todo.id === id);
      if (index !== -1) {
        this.todosData.splice(index, 1);
      }
    },

    // moveToCompleted(todo) {
    //   this.completedTasks.unshift(todo);

    //   const index = this.todosData.findIndex((t) => t.id === todo.id);
    //   if (index !== -1) {
    //     this.todosData.splice(index, 1);
    //   }
    // },

    // complete(id) {
    //   const updated_todos = this.todosData.map((todo) =>
    //     todo.id == id ? Object.assign(todo, { completed: true }) : todo
    //   );
    //   this.todosData = updated_todos;
    // },
    // deleteTodo(id) {
    //   let deleted_todos = this.todosData.filter((todo) => todo.id != id);
    //   this.todosData = deleted_todos;
    // },

    // moveToCompleted(todo) {
    //   this.completedTasks.unshift(todo);

    // const index = this.todosData.findIndex((t) => t.id === todo.id);
    // if (index !== -1) {
    //   this.todosData.splice(index, 1);
    // }
    // },
  },
};
</script>

<template>
<div id="app">
    <div class="section">
      <div class="container">
        <div class="wrapper">
          <TodosComponent
            :todos="todosData"
            @addTodo="addTodo"
            @onDelete="deleteTodo"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
