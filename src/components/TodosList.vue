<template>
    <section class="todoapp">
        <header class="header">
            <h1>Ma liste de taches</h1>
            <input type="text" @keyup.enter="addTodo" v-model="newTodo" placeholder="Tapez et presser entrer pour ajouter une tache" class="new-todo">
        </header>

        <div class="main">
            <input type="checkbox" class="toggle-all" v-model="allToggle">
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed}">
                    <div class="view">
                        <input class="toggle" type="checkbox" v-model="todo.completed">
                        <label> {{ todo.name }} </label>
                        <button @click.prevent="deleteTodo(todo)" class="destroy"></button>
                    </div>
                </li>
            </ul>
        </div>

        <footer class="footer" v-show="hasTodo">
            <span class="todo-count">
                <strong> {{ remaining }} </strong> Taches à faire
            </span>
            <ul class="filters">
                <li> <a href="#" :class="{selected: filter === 'all'}" @click.prevent="changeFilter('all')">Toutes</a> </li>
                <li> <a href="#" :class="{selected: filter === 'todo'}" @click.prevent="changeFilter('todo')">A faire</a> </li>
                <li> <a href="#" :class="{selected: filter === 'done'}" @click.prevent="changeFilter('done')">Faites</a> </li>
            </ul>
        </footer>
    </section>
</template>

<script>

    export default {

        data: function () {
            return {
                todos: [
                ],
                newTodo: '',
                filter: 'all'
            }
        },

        methods: {
            addTodo (){
                this.todos.push({
                    name: this.newTodo,
                    completed: false
                });
                this.newTodo = '';
            },

            changeFilter (filter){
                this.filter = filter;
            },

            deleteTodo (td){
                this.todos = this.todos.filter(todo => todo.name !== td.name);
            }
        },

        computed: {
            remaining (){
                return this.todos.filter(todo => !todo.completed).length;
            },
            allToggle (){
                return this.todos.every(todo => todo.completed);
            },
            filteredTodos (){
                if (this.filter === 'all') {
                    return this.todos;
                } else if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed);
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed);
                }
                return this.todos;
            },
            hasTodo (){
                return this.todos.length > 0;
            }
        }
    }
</script>

<style src="../styles/todos.css"></style>