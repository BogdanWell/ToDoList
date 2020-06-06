<template>
  <div>
    <input 
    type="text" 
    class="todo-input" 
    placeholder="what needs to be done"
    v-model="newToDo" @keyup.enter="addToDo">
    <div 
    v-for="(todo, index) in todos" :key="todo.id" 
    class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed">
        <div
          v-if="!todo.editing"
          @dblclick="editToDo(todo)"
          class="todo-item-label"
          :class="{ completed : todo.completed}"
        >{{todo.title}}</div>
        <input
          v-else
          class="todo-item-edit"
          type="text"
          v-model="todo.title"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
          @keyup.esc="cancelEdit(todo)"
          v-focus
        />
      </div>
      <div class="remove-item" @click="removeToDo(index)">&times;</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "todolist",
  data() {
    return {
      newToDo: "",
      idForToDo: 3,
      beforeEditCache: "",
      todos: [
        {
          id: 1,
          title: "First to do",
          completed: false,
          editing: false
        },

        {
          id: 2,
          title: "Second to do",
          completed: false,
          editing: false
        }
      ]
    };
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addToDo() {
      if (this.newToDo.trim().length == 0) return;
      this.todos.push({
        id: this.idForToDo,
        title: this.newToDo,
        completed: false
      });
      this.newToDo = "";
      this.idForToDo++;
    },
    editToDo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },
    removeToDo(index) {
      this.todos.splice(index, 1);
    }
  }
};
</script>


<style>
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 12px;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;

  &:hover {
    color: black;
  }
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: "Avenir", "Helvetica", "Arial", sans-serif;

  &:focus {
    outline: none;
  }
}

.completed {
  text-decoration: line-through;
  color: grey;
}
</style>
