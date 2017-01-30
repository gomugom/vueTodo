<template>
<li :class="[ 'todo-item', {
    'editing': editing,
    'completed': todo.done
  }]"
>
  <div class="toggle" @click="toggleTodo" />
  <div class="todo-item__view">
    <div
      class="todo-item__view__text"
      @dblclick="startEdit"
    >{{ todo.text }}</div>
    <button class="todo-item__destroy" @click="deleteTodo" />
  </div>
  <input
    ref="inputRef"
    class="todo-item__edit"
    type="text"
    @blur="cancelEdit"
    @keyup.13="saveTodo"
  />
</li>
</template>

<script>
export default {
  name: 'todo',
	props: {
		todo: Object,
    editing: Boolean
	},
  methods: {
    deleteTodo () {
      this.$emit('deleteTodo', this.todo.id)
    },
    toggleTodo () {
      this.$emit('toggleTodo', this.todo.id)
    },
    startEdit () {
      this.$emit('startEdit', this.todo.id)
      this.$refs.inputRef.value = this.todo.text
      setTimeout(() => this.$refs.inputRef.focus())
    },
    cancelEdit () {
      this.$emit('cancelEdit')
    },
    saveTodo (e) {
      const tg = e.target
      if (tg.value.length) {
        this.$emit('saveTodo', this.todo.id, tg.value)
      }
    }
  }
}
</script>

<style>
.todo-item {
	position: relative;
	font-size: 24px;
	padding-left: 45px;
	border-bottom: 1px solid #ededed;
}

.todo-item:last-child {
	border-bottom: none;
}

.todo-item.editing .todo-item__edit {
	display: block;
	width: 100%;
	padding: 13px 15px;
	font-size: 24px;
}

.todo-item.editing .todo-item__view {
	display: none;
}

.todo-item .toggle {
	text-align: center;
	width: 40px;
	/* auto, since non-WebKit browsers doesn't support input styling */
	height: auto;
	position: absolute;
	top: 0;
	bottom: 0;
	left: 0;
	margin: auto 0;
	border: none; /* Mobile Safari */
	-webkit-appearance: none;
	appearance: none;
	cursor: pointer;
}

.todo-item .toggle:after {
	content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="-10 -50 120 170"><circle cx="50" cy="50" r="50" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
}

.todo-item.completed .toggle:after {
	content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" viewBox="-10 -50 120 170"><circle cx="50" cy="50" r="50" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
}

.todo-item__view__text {
	white-space: pre-line;
	word-break: break-all;
	padding: 15px 60px 15px 15px;
	display: block;
	line-height: 1.2;
	transition: color 0.4s;
}

.todo-item.completed .todo-item__view__text {
	color: #d9d9d9;
	text-decoration: line-through;
}

.todo-item__destroy {
	display: none;
	position: absolute;
	top: 0;
	right: 10px;
	bottom: 0;
	width: 40px;
	height: 40px;
	margin: auto 0;
	font-size: 30px;
	color: #cc9a9a;
	margin-bottom: 11px;
	transition: color 0.2s ease-out;
}

.todo-item__destroy:hover {
	color: #af5b5e;
}

.todo-item__destroy:after {
	content: '×';
}

.todo-item:hover .todo-item__destroy {
	display: block;
}

.todo-item__edit {
	display: none;
}

.todo-item.editing:last-child {
	margin-bottom: -1px;
}
</style>
