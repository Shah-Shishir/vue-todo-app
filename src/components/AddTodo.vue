<template>
  <div class="wrapper">
    <div class="input-todo">
      <i v-if="todos.length" class="fa fa-angle-down"></i>
      <form @submit.prevent="addTodo">
        <input
            type="text"
            name="input-todo"
            v-model="inputTodo"
            placeholder="What needs to be done?"
            autocomplete="off"
        />
      </form>
    </div>
    <div v-if="todoState === 'all'" class="todo-list">
      <div class="single-todo" v-bind:key="todo.id" v-for="todo in todos">
        <span class="check" v-on:click="markAsComplete(todo.id)">
            <i class="fa fa-check"></i>
        </span>
        <div class="todo-info d-ib">
          <label class="todo-name">
            <input type="text" v-model="todo.name" v-bind:class="{'completed': todo.isCompleted}"/>
            <span v-on:click="deleteTodo(todo.id)" class="delete-icon">x</span>
          </label>
        </div>
      </div>
    </div>
    <div v-if="todoState === 'active'" class="todo-list">
      <div class="single-todo" v-bind:key="todo.id" v-for="todo in activeTodos">
        <span class="check" v-on:click="markAsComplete(todo.id)">
            <i class="fa fa-check"></i>
        </span>
        <div class="todo-info d-ib">
          <label class="todo-name">
            <input type="text" v-model="todo.name" v-bind:class="{'completed': todo.isCompleted}"/>
            <span v-on:click="deleteTodo(todo.id)" class="delete-icon">x</span>
          </label>
        </div>
      </div>
    </div>
    <div v-if="todoState === 'completed'" class="todo-list">
      <div class="single-todo" v-bind:key="todo.id" v-for="todo in completedTodos">
        <span class="check" v-on:click="markAsComplete(todo.id)">
            <i class="fa fa-check"></i>
        </span>
        <div class="todo-info d-ib">
          <label class="todo-name">
            <input type="text" v-model="todo.name" v-bind:class="{'completed': todo.isCompleted}"/>
            <span v-on:click="deleteTodo(todo.id)" class="delete-icon">x</span>
          </label>
        </div>
      </div>
    </div>
    <div v-if="todos.length" class="todo-task">
      <div class="active-todo-count">
        {{ todos.length-completedTodoCount }} <span v-if="todos.length-completedTodoCount <= 1"> item </span>
        <span v-if="todos.length-completedTodoCount > 1"> items </span> left
      </div>
      <div class="navigation">
        <a class="page-link" id="a-1" v-on:click="changeTodoState(1, 'all')">All</a>
        <a class="page-link" id="a-2" v-on:click="changeTodoState(2,'active')">Active</a>
        <a class="page-link" id="a-3" v-on:click="changeTodoState(3,'completed')">Completed</a>
      </div>
      <div v-bind:class="{'hidden': !completedTodoCount}" class="clear">
        <a id="a-4" v-on:click="changeTodoState(4)">Clear Completed</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddTodo',
  data() {
    return {
      completedTodoCount: 0,
      inputTodo: '',
      todos: [],
      completedTodos: [],
      activeTodos: [],
      todoState: 'all',
      prev: null
    }
  },
  methods: {
    addTodo() {
      let newTodo = {
        id: this.todos.length,
        name: this.inputTodo,
        isCompleted: false
      }
      this.todos = this.activeTodos = [...this.todos, newTodo];
      this.inputTodo = '';
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    markAsComplete(id) {
      this.activeTodos = this.activeTodos.filter(todo => todo.id !== id);
      this.todos[id].isCompleted = !this.todos[id].isCompleted;
      this.completedTodos.push(this.todos[id]);
      ++this.completedTodoCount;
    },
    changeTodoState(n, state) {
      if (n === 4) {
        this.completedTodos = [];
      }
      if (state) {
        this.todoState = state;
      }
      if (this.prev !== null) {
        document.querySelector('#a-' + this.prev).classList.remove('active');
      }
      document.querySelector('#a-' + n).classList.add('active');
      this.prev = n;
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  border: 1px solid #eaeaea;
  width: 440px;
  margin: auto;

  .active {
    border: 1px solid #eaeaea;
    border-radius: 5px;
  }

  .d-none {
    display: none;
  }

  .d-ib {
    display: inline-block;
  }

  .hidden {
    visibility: hidden;
  }

  .completed {
    color: #d0caca;
    text-decoration: line-through;
  }

  .input-todo {
    padding: 0 15px;
    border-bottom: 1px solid #f7f0f0;

    i {
      font-size: 20px;
      color: #eaeaea;
    }

    form {
      display: inline-block;

      input {
        padding: 15px;
        font-size: 16px;
        width: 100%;
        border: none;

        &:focus {
          outline: none !important;
        }

        &::placeholder {
          font-size: 16px;
          font-style: italic;
          color: #eaeaea;
        }
      }
    }
  }

  .todo-list {
    .single-todo {
      color: #4e4949;
      border-bottom: 1px solid #f7f4f4;

      &:hover {
        .delete-icon {
          display: inline !important;
        }
      }

      .check {
        margin-left: 10px;

        .fa-check {
          color: #8cb78c;
          border: 1px solid #eaeaea;
          border-radius: 50%;
          padding: 2px;
          font-size: 16px;
          cursor: pointer;
          visibility: hidden;
        }

        &:hover {
          .fa-check {
            visibility: visible;
          }
        }
      }

      .todo-info {
        width: 86%;
        .todo-name {
          input {
            font-size: 16px;
            border: none;
            padding: 12px 10px;
            width: 90%;

            &:focus {
              border: 2px solid #eaeaea;
              outline: none !important;
              width: 100%;
            }
          }

          &:focus-within {
            .delete-icon {
              display: none !important;
            }
          }
        }

        .delete-icon {
          cursor: pointer;
          color: #d6c2c2;
          font-size: 18px;
          font-weight: bold;
          display: none;
        }
      }
    }
  }

  .todo-task {
    display: flex;
    color: #4c4949;
    font-size: 12px;
    padding: 10px;

    a {
      cursor: pointer;
      text-decoration: none;
      color: #4c4949;
      margin-left: 10px;
      padding: 4px;
      border-radius: 5px;
      &:hover {
        border: 1px solid #eaeaea;
      }
    }

    .active-todo-count {
      flex: 1;
    }
    .navigation {
      flex: 1;
    }
    .clear {
      flex: 1;
      a {
        text-decoration: none;
        color: #4c4949;
        margin-left: 10px;
        padding: 4px;
        &:hover {
          text-decoration: underline;
          border: none;
        }
      }
    }
  }
}
</style>