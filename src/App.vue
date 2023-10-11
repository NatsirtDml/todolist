<template>
  <div id="app">
    <div class="Header">
      <h1>To Do List Doums</h1>
      <img
        class="Logo"
        src="https://c8.alamy.com/comp/2BYBEAY/doodle-to-do-list-icon-or-logo-hand-drawn-with-thin-black-line-2BYBEAY.jpg"
        alt="AU BOULOT"
        width="200"
        height="200"
      />
    </div>
    <input type="text" name="todo" id="todo" placeholder="Ajouter un ToDo" />
    <button type="submit" @click="addTodo">Add</button>
    <div>
      <ToDo
        v-for="element in list"
        :key="element.id"
        :id="element.id"
        :text="element.todo"
        :checked="element.checked"
        @delete="deleteToDo"
        @checked="check"
      ></ToDo>
    </div>
  </div>
</template>

<script>
import ToDo from "./components/ToDo.vue";
import axios from "axios";
export default {
  data() {
    return {
      newToDo: "",
      list: [],
    };
  },

  name: "App",
  components: {
    ToDo,
  },

  methods: {
    // addTodo() {
    //   const newToDo = document.getElementById("todo").value;
    //   this.list.push(newToDo);
    // },

    async addTodo() {
      const text = document.getElementById("todo").value;
      const response = await axios.put("http://localhost:3000/todo", {
        todo: text,
      });
      this.list.push({ id: response.data[0], todo: text, checked: 0 });
    },

    async deleteToDo(id) {
      await axios.delete(`http://localhost:3000/todo/${id}`);
      const textDelete = this.list.findIndex((element) => element.id === id);
      this.list.splice(textDelete, 1);
    },

    async check({ id, value }) {
      await axios.post(`http://localhost:3000/todo/check/${id}/${value}`);
    },
  },

  async mounted() {
    const response = await axios.get("http://localhost:3000/todo");
    this.list = response.data;
  },
};
</script>

<style>
#app {
  font-family: "Comic Sans MS", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.Header {
  display: flex;
  align-items: center;
  justify-content: center;
}
.Logo {
  position: absolute;
  right: 0%;
  top: 0.5%;
}
</style>
