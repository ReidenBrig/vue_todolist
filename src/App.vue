<template>
  <div id="app">
    <h1 class="title">
      {{ title }}
    </h1>

    <div class="todo">
      <input v-model="todo" type="text" placeholder="type todo..."/>
      <button @click="addTodo(id++)">Add todo</button>

      <div class="todo__box" v-for="(todo,i) in todos" :key="todo.id">
        <p>
          <span class="todo__id">{{ i + 1 }}.</span>
          <span
              class="todo__text"
              :class="{todo__text_isShow: todo.isComplete}"

          >
            {{ todo.text }}
          </span>
          <input v-model="todo.isComplete" type="checkbox"/>
          <span class="todo__delete" @click="removeTodo(i)">x</span>
        </p>
      </div>
      <hr>
      <p>
        Список задач: {{ todos.length }}
      </p>
    </div>


  </div>
</template>

<script>


export default {

  data() {
    return {
      title: 'ToDo app',
      todo: '',
      todos: [],
      isDone: false,
      id: 0
    };
  },
  async mounted() {
    const data = await localStorage.getItem('todos');
    data ? this.todos = JSON.parse(data) : null;
  },
  methods: {
    addTodo() {
      if (this.todo != '') {
        this.todos.push({
          id: this.id,
          text: this.todo,
          isComplete: this.isDone
        })

        localStorage.setItem('todos', JSON.stringify(this.todos))
      }

      this.todo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  text-align: center;
}

.todo {
  margin: 0 auto;
  padding: 30px;
  width: 300px;
  height: 100%;
  background-color: #bdbdbd;
  outline: 1px solid black;
  border-radius: 10px;
  min-height: 500px;

  &__box {
    margin-top: 10px;
    min-width: 250px;
    border: #2c3e50 1px solid;
    border-radius: 10px;
  }

  &__id {
    color: #2c3e50;
  }

  &__text {
    font-size: 15px;
    color: #2c3e50;
    text-transform: capitalize;

    &_isShow {
      color: grey;
      text-decoration: line-through;
    }
  }

  &__delete {
    padding: 0 5px;
    border: 1px solid #2c3e50;
    border-radius: 3px;
    margin-right: 10px;
    float: right;
  }

}

</style>
