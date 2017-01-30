<template>
<div class="app-main">
  <div
		:class="['toggle-all', {
			'checked': todoList.every(({done})=> done)
		}]"
		@click="toggleAll"
	/>
  <ul class="todo-list">
    <todo
			v-for="todo in todoList"
			:todo="todo"
			:editing="editingId === todo.id"
    	@toggleTodo="toggleTodo"
			@deleteTodo="deleteTodo"
			@startEdit="startEdit"
			@cancelEdit="cancelEdit"
			@saveTodo="saveTodo"
		/>
  </ul>
</div>
</template>

<script>
import Todo from './Todo'
export default {
  name: 'todo-list',
  props: {
    todoList: Array
  },
  components: {
    Todo
  },
	data () {
		return {
			editingId: null
		}
	},
	methods: {
		deleteTodo (id) {
			this.$emit('deleteTodo', id)
		},
		toggleTodo (id) {
			this.$emit('toggleTodo', id)
		},
		toggleAll () {
			this.$emit('toggleAll')
		},
		startEdit (id) {
			this.editingId = id
		},
		cancelEdit () {
			this.editingId = null
		},
		saveTodo (id, newText) {
			this.$emit('saveTodo', id, newText)
			this.cancelEdit()
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.app-main {
	position: relative;
	z-index: 2;
	border-top: 1px solid #e6e6e6;
}
.toggle-all {
	position: absolute;
	top: -55px;
	left: -12px;
	width: 60px;
	height: 34px;
	text-align: center;
	border: none; /* Mobile Safari */
	cursor: pointer;
	-webkit-transform: rotate(90deg);
	transform: rotate(90deg);
	-webkit-appearance: none;
	appearance: none;
}

.toggle-all:before {
	content: '❯';
	font-size: 22px;
	color: #e6e6e6;
	padding: 10px 27px 10px 27px;
}

.toggle-all.checked:before {
	color: #737373;
}

.todo-list {
	margin: 0;
	padding: 0;
	list-style: none;
}

</style>
