<template>
  <div class="columns is-multiline">
    <div class="column is-6 is-offset-3">
      <div class="box">
        <form @submit.prevent="onSubmit">
          <label class="label">Todo List</label>
          <div class="field">
            <div class="control">
              <input
                class="input"
                type="text"
                placeholder="Add your todo list here!!!!!"
                v-model="title"
              />
            </div>
          </div>
        </form>
      </div>
    </div>

    <div class="new_column" v-for="todo in todos" :key="todo.id">
      <div
        class="box todobox"
        @click="toggleComplete(todo.id)"
        :class="{ completed: todo.completed }"
      >
        <button class="edit" @click.prevent="editTodo(todo.id)">Edit</button>
        <span id="task">{{ todo.title }}</span>
        <button
          class="remove"
          v-if="todo.completed"
          @click="$emit('onDelete', todo.id)"
        >
           delete
        </button>
      </div>
    </div>
  </div>
</template>


<script>
import shortid from "shortid";

export default {
  props: {
    todos: Array,
    // completedTasks: Array,
  },
  data() {
    return {
      title: "",
      editingTodoId: null,
    };
  },
  methods: {
    onSubmit() {
      if (this.title.trim() !== "") {
        if (this.editingTodoId !== null) {
          const editedTodoIndex = this.todos.findIndex(
            (todo) => todo.id === this.editingTodoId
          );
          const editedTodo = {
            id: this.editingTodoId,
            title: this.title,
            completed: this.todos[editedTodoIndex].completed,
          };
          this.todos.splice(editedTodoIndex, 1, editedTodo);
          this.editingTodoId = null;
        } else {
          const newTodo = {
            id: shortid.generate(),
            title: this.title,
            completed: false,
          };
          this.$emit("addTodo", newTodo);
        }

        this.title = "";
      }
    },

    editTodo(todoId) {
      this.editingTodoId = todoId;
      const todoToEdit = this.todos.find((todo) => todo.id === todoId);
      this.title = todoToEdit.title;
    },

    complete(id) {
      const todoToComplete = this.todos.find((todo) => todo.id === id);
      todoToComplete.completed = !todoToComplete.completed;
      // this.$emit("onComplete", id);
    },
    
    toggleComplete(id) {
      const todoToToggle = this.todos.find((todo) => todo.id === id);
      todoToToggle.completed = !todoToToggle.completed;

      // Automatically move the completed task to the CompletedTasksComponent
      if (todoToToggle.completed) {
        this.$emit("moveToCompleted", { ...todoToToggle });
      }
    },

    addTodo() {
      if (this.title.trim() !== "") {
        if (this.editingTodoId !== null) {
          const editedTodoIndex = this.todos.findIndex(
            (todo) => todo.id === this.editingTodoId
          );
          this.todos[editedTodoIndex].title = this.title;
          this.editingTodoId = null;
        } else {
          const newTodo = {
            id: this.todos.length + 1,
            title: this.title,
            completed: false,
          };
          this.todos.push(newTodo);
        }
        this.title = "";
      }
    },
  },
};
</script>


<style scoped>
.completed {
  text-decoration: line-through;
  color: red;
}

.label {
  color: black;
  display: flex;
  justify-content: center;
  margin: 0 auto;
  align-items: center;
  font-size: 50px;
  font-family: "Cormorant", serif;
  font-weight: 600;
}
.section {
  display: flex;
  margin: 0 auto;
  justify-content: center;
}

.wrapper {
  height: 80vh;
}
.container {
  background-color: rgba(182, 143, 143, 0.972);
  height: 50rem;
  width: 50%;
  border-radius: 10px;
  margin: 5%;
}

#task {
  background-color: rgb(29, 133, 144);
  width: 100%;
  font-size: 18px;
  margin: 30px 60px;
  padding: 5px 20px;
  border-radius: 8px;
}

.todobox {
  transition: all 0.5s;
  margin: 10px auto;
  padding: 14px;
  background-color: rgb(94, 156, 58);
  border-radius: 5px;
  width: 65%;
  justify-content: center;
  align-content: center;
}

.todobox.completed {
  text-decoration: line-through;
}
.todobox:hover {
  width: 50%;
  cursor: pointer;
  transform: scale(1.1);
  color: rgb(231, 235, 4);
}
.field {
  background-color: rgb(78, 131, 109);
  width: 65%;
  height: 90px;
  margin: 10px auto;
  padding: 11px 15px;
  border-radius: 5px;
}
.input {
  background-color: rgb(251, 242, 251);
  width: 55%;
  font-size: 14px;
  margin: 8px 16px;
  padding: 10px 20px;
  border-radius: 8px;
  border-style: none;
  height: 50px;
  justify-content: center;
  align-items: center;
}

/* .new_column {
  background-color: orange;
  display: flex;
  align-items: center;
  justify-content: space-between;
} */
.edit{
  background-color: rgba(12, 167, 12, 0.856);
  color: white;
  padding: 6px;
  border-radius: 4px;
  border-style: none;
}
.remove{
  /* display: flex; */
  align-items: center;
  background-color: red;
justify-content: flex-end;
border-radius: 2px;
padding: 6px;
border-style: none;
}
</style>