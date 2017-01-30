<template>
<div class="todo-app">
  <app-header
		@addTodo="addTodo"
	/>
  <todo-list
		:todo-list="viewTodoList"
		@deleteTodo="deleteTodo"
  	@toggleTodo="toggleTodo"
		@toggleAll="toggleAll"
		@saveTodo="saveTodo"
	/>
  <app-footer
		:active-length="todoList.filter(todo => todo.done).length"
		:filter="currentRoute"
		@deleteCompleted="deleteCompleted"
		@routeChange="routeChange"
	/>
</div>
</template>

<script>
import AppHeader from './AppHeader'
import TodoList from './TodoList'
import AppFooter from './AppFooter'
import axios from 'axios'

const axiosApi = axios.create({
  baseURL: 'http://localhost:2403/todos',
  timeout: 1000,
  responseType: 'json'
})

const ax = ({
  method = 'post',
  url = '/',
  data,
  res,
  rej = err => { console.error(err) }
}) => {
  if (data) return axiosApi[method](url, data).then(res).catch(rej)
  return axiosApi[method](url).then(res).catch(rej)
}

export default {
  name: 'app',
  components: {
    AppHeader,
    TodoList,
    AppFooter
  },
	data () {
    return {
			currentRoute: window.location.pathname,
      todoList: []
    }
  },
	computed: {
		viewTodoList () {
			return this.todoList.filter(todo => {
				if (
					this.currentRoute === '/' ||
					this.currentRoute === '/active' && !todo.done ||
					this.currentRoute === '/completed' && todo.done
				) return true
				return false
			})
		}
	},
  beforeMount () {
    ax({
			method: 'get',
			res: ({data}) => { this.todoList = data }
		})
  },
	methods: {
		routeChange (route) {
			this.currentRoute = route
		},
		addTodo (text) {
			ax({
				data: { text },
				res: ({data: { id, text, done }}) => {
					this.todoList = [...this.todoList, { id, text, done }]
				}
			})
		},
		deleteTodo (id) {
			ax({
				method: 'delete',
				url: `/${id}`,
				res: () => {
					const deleteIndex = this.todoList.findIndex(v => v.id === id)
					this.todoList.splice(deleteIndex, 1)
				}
			})
		},
		toggleTodo (id) {
			const toggleIndex = this.todoList.findIndex(v => v.id === id)
			ax({
				method: 'put',
				url: `/${id}`,
				data: { done: !this.todoList[toggleIndex].done },
				res: ({ data }) => {
					this.todoList.splice(toggleIndex, 1, data)
				}
			})
		},
		toggleAll () {
			const newToggle = !this.todoList.every(v => v.done)
			axios.all(
				this.todoList.map(
					todo => axiosApi.put(todo.id, {done: newToggle})
				)
			).then(res => {
				this.todoList = res.map(v => v.data)
			})
		},
		deleteCompleted () {
			const deleteList = this.todoList.filter(todo => todo.done)
			const newTodoList = this.todoList.filter(todo => !todo.done)
			axios.all(
				deleteList.map(
					todo => axiosApi.delete(todo.id)
				)
			).then(() => {
				this.todoList = newTodoList
			})
		},
    saveTodo (id, newText) {
			ax({
				method: 'put',
				url: `/${id}`,
				data: { text: newText },
				res: ({ data }) => {
					this.todoList.splice(this.todoList.findIndex(v => v.id === id), 1, data)
				}
			})
    },
		selectFilter (filter) {
			this.filter = filter
		}
	}
}
</script>

<style>
* {
	box-sizing: border-box;
}
button {
	margin: 0;
	padding: 0;
	border: 0;
	background: none;
	font-size: 100%;
	vertical-align: baseline;
	font-family: inherit;
	font-weight: inherit;
	color: inherit;
	-webkit-appearance: none;
	appearance: none;
	-webkit-font-smoothing: antialiased;
	-moz-font-smoothing: antialiased;
	font-smoothing: antialiased;
}

body {
	font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
	line-height: 1.4em;
	background: #f5f5f5;
	color: #4d4d4d;
	min-width: 230px;
	max-width: 550px;
	margin: 0 auto;
	-webkit-font-smoothing: antialiased;
	-moz-font-smoothing: antialiased;
	font-smoothing: antialiased;
	font-weight: 300;
}

button,
input[type="checkbox"] {
	outline: none;
}

.hidden {
	display: none;
}

.todo-app {
  background: #fff;
  margin: 130px 0 40px 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

.todo-app input::-webkit-input-placeholder {
	font-style: italic;
	font-weight: 300;
	color: #e6e6e6;
}

.todo-app input::-moz-placeholder {
	font-style: italic;
	font-weight: 300;
	color: #e6e6e6;
}

.todo-app input::input-placeholder {
	font-style: italic;
	font-weight: 300;
	color: #e6e6e6;
}

.new-todo,
.edit-todo {
  position: relative;
	margin: 0;
	width: 100%;
	font-size: 24px;
	font-family: inherit;
	font-weight: inherit;
	line-height: 1.4em;
	border: 0;
	outline: none;
	color: inherit;
	padding: 6px;
	border: 1px solid #999;
	box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
	box-sizing: border-box;
	-webkit-font-smoothing: antialiased;
	-moz-font-smoothing: antialiased;
	font-smoothing: antialiased;
}

@media screen and (-webkit-min-device-pixel-ratio:0) {
	.toggle-all,
	.todo-item .toggle {
		background: none;
	}

	.todo-item .toggle {
		height: 40px;
	}

	.toggle-all {
		-webkit-transform: rotate(90deg);
		transform: rotate(90deg);
		-webkit-appearance: none;
		appearance: none;
	}
}

@media (max-width: 430px) {
	.footer {
		height: 50px;
	}

	.todo-filters {
		bottom: 10px;
	}
}
</style>
