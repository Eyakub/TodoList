<template>
    <div id="app">
        <Header></Header>
        <AddTodo v-on:add-todo="addTodo"></AddTodo>
        <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"></Todos>
    </div>
</template>

<script>
import Todos from "./components/Todos";
import Header from "./components/layout/Header";
import AddTodo from "./components/AddTodo";
import axios from "axios";

export default {
    name: "app",
    components: {
        Header,
        Todos,
        AddTodo
    },
    data() {
        return {
            todos: [
                {
                    id: 1,
                    title: "Todo One",
                    completed: true
                },
                {
                    id: 2,
                    title: "Todo two",
                    completed: false
                },
                {
                    id: 3,
                    title: "Todo Three",
                    completed: true
                }
            ]
        };
    },
    methods: {
        deleteTodo: function(id) {
            this.todos = this.todos.filter(todo => todo.id !== id);
        },
        addTodo: function(newTodo) {
            const { title, completed } = newTodo;
            // this.todos = [...this.todos, newTodo];
            axios
                .post("https://jsonplaceholder.typicode.com/todos", {
                    title,
                    completed
                })
                .then(response => (this.todos = [...this.todos, response.data]))
                .catch(error => console.log(error));
        }
    },
    created() {
        axios
            .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
            .then(response => (this.todos = response.data))
            .catch(error => console.log(error));
    }
};
</script>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
}

.btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
}

.btn:hover {
    background: #666;
}
</style>
