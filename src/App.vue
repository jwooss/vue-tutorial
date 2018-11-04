<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @updateTodo="updateTodo" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
    import TodoHeader from './components/TodoHeader';
    import TodoInput from './components/TodoInput';
    import TodoList from './components/TodoList';
    import TodoFooter from './components/TodoFooter';

    export default {
        data() {
            return {
                todoItems: []
            }
        },
        methods: {
            addTodo(todoItem) {
                // 로컬 스토리지에 추가하는 로직
                let timestamp = new Date().getTime();

                localStorage.setItem(timestamp, todoItem);
                this.todoItems.push({
                    'key': timestamp,
                    'value': todoItem
                });
            },
            clearAll() {
                localStorage.clear();
                this.todoItems = [];
            },
            removeTodo(todoItem, index) {
                //console.log(todoItem);
                localStorage.removeItem(todoItem.key);
                this.todoItems.splice(index, 1);
            },
            updateTodo(todoItem, index) {
                localStorage.setItem(todoItem.value, todoItem.value);
                this.todoItems[index] = todoItem;
            }
        },
        created() {
            if (localStorage.length > 0) {
                for (var i = 0; i < localStorage.length; i++) {
                    this.todoItems.push({
                        'key': localStorage.key(i),
                        'value': localStorage.getItem(localStorage.key(i))
                    });
                }
            }
        },
        components: {
            'TodoHeader': TodoHeader,
            'TodoInput': TodoInput,
            'TodoList': TodoList,
            'TodoFooter': TodoFooter,
        }
    }
</script>

<style>
  body {
    text-align: center;
    background-color: #F6F6F8;
  }

  input {
    border-style: groove;
    width: 200px;
  }

  button {
    border-style: groove;
  }

  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
  }
</style>