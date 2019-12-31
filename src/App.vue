<template>
  <div>
    <app-header v-bind:title="title" v-on:changeTitle="updateTitle($event)"></app-header>
    <TodoApp/>
    <app-footer v-bind:title="title"></app-footer>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import TodoApp from './components/TodoApp'

export default {
  name: "app",
  components: {
    "app-header": Header,
    "app-footer": Footer,
    TodoApp
  },
  data: function() {
    return {
      title: "Todo App"
    };
  },
  methods:{
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
