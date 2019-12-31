<template>
  <div class="container">
    <div class="todo-input">
      <input
        type="text"
        name="todo"
        id="todo"
        placeholder="Enter Your Todo Here..."
        v-model="newTodo.todo"
        @keyup.enter="addTodo"
      />
    </div>

    <div class="container">
      <h2>Todo List</h2>
      <div class="List">
        <ul>
          <li
            v-for="(todo,id) in getTodos"
            :key="todo.index"
            v-on:dblclick="deleteTodo(id)"
          >{{todo.todo}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { openDB, deleteDB } from "idb";

export default {
  data: function() {
    return {
      newTodo: {
        todo: ""
      },
      dbTodo: {
        todo: ""
      },
      getTodos: [],
      savedTodos: []
    };
  },
  methods: {
    addTodo: async function() {
      try {
        if (!this.newTodo.todo) {
          alert("Enter a Todo");
          return;
        }
        if (!navigator.onLine) {
          if (!("indexedDB" in window)) {
            console.warn("IndexedDB not supported");
            return;
          }
          console.log("Hello");
          console.log(navigator.onLine);
          const dbName = "mydbname";
          const storeName = "store2";
          const version = 1; //versions start at 1

          const db = await openDB(dbName, version, {
            upgrade(db, oldVersion, newVersion, transaction) {
              const store = db.createObjectStore(storeName);
            }
          });
          const tx = db.transaction(storeName, "readwrite");
          const store = await tx.objectStore(storeName);

          const value = await store.put(this.newTodo.todo, this.newTodo.todo);
          await tx.dones;

          this.savedTodos = await db
            .transaction(storeName)
            .objectStore(storeName)
            .getAll();
          console.log("ITEMS", this.savedTodos);
        } else {
          // const items = await db.transaction(storeName).objectStore(storeName).getAllKeys()

          // console.log("ITEMS",items)
          console.log(this.savedTodos);
          if (this.savedTodos.length) {
            console.log(this.savedTodos.length);
            this.savedTodos.forEach(element => {
              console.log("ELE", element);
              this.dbTodo.todo = element;
              console.log(this.dbTodo);
              axios
                .post("https://vuetodoapp-f56da.firebaseio.com/todo.json", {
                  todo: element
                })
                .then(() => {
                  // this.newTodo=''
                });
              // for(var i=0;i<100;i++){

              // }
            });
            const dbName = "mydbname";
            await deleteDB(dbName);
            this.savedTodos = [];
          } else {
            console.log("Kuch");
            axios
              .post(
                "https://vuetodoapp-f56da.firebaseio.com/todo.json",
                this.newTodo
              )
              .then(() => {
                this.newTodo = "";
              });
          }
        }
      } catch (error) {}
    },
    deleteTodo: function(todo) {
      console.log("todo==", todo);
      axios
        .delete(`https://vuetodoapp-f56da.firebaseio.com/todo/${todo}.json`)
        .then(() => {});

      // alert(index)
      // this.todos.splice(index,1)
    }
  },
  created: function() {
    try {
      if (!navigator.onLine) {
        return;
      }
      axios
        .get("https://vuetodoapp-f56da.firebaseio.com/todo.json")
        .then(res => {
          console.log("KEY", res);
          // this.getTodos=res.data
          return res;
        })
        .then(data => {
          console.log("Data==>", data);
          var todoArray = [];
          for (let key in data.data) {
            console.log(key);
            // console.log(data.data[key]);
            data.data[key].id = key;
            todoArray.push(data.data[key]);
          }
          console.log("ARRAY", todoArray);
          this.getTodos = todoArray;
        });
    } catch (error) {}
  },
  beforeUpdate: function() {
    try {
      if (!navigator.onLine) {
        return;
      }
      axios
        .get("https://vuetodoapp-f56da.firebaseio.com/todo.json")
        .then(res => {
          // console.log(res)
          this.getTodos = res.data;
          // return res.json();
        });
    } catch (error) {}
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  text-align: center;
}
input {
  width: 30%;
  height: 30px;
  margin: 2%;
}
li {
  list-style: none;
  font-size: 30px;
  width: 90%;
  height: 50px;
}
.List {
  text-align: left;
  width: 40%;
  border: 1px solid black;
  margin-left: 30%;
  overflow-y: auto;
  max-height: 550px;
  height: 550px;
}
</style>
