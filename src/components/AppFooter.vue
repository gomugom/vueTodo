<template>
<div class="app-footer">
  <span class="todo-count">
    <strong>{{ activeLength }}</strong> <span>{{ itemMark }}</span> left
  </span>
  <ul class="todo-filters">
    <li v-for="filterName in ['/', '/active', '/completed']">
      <a
				:href="filterName"
				:class="{'selected' : filter === filterName }"
				@click="pushState"
			>
				{{filterName.slice(1).replace(/^\w/, v => v.toUpperCase()) || 'All'}}
			</a>
    </li>
  </ul>
  <button
		class="todo-delete-completed"
		@click="deleteCompleted"
	>
    Delete Completed
  </button>
</div>
</template>

<script>
  export default {
    name: 'app-footer',
    props: {
      activeLength: Number,
			filter: String
    },
		computed: {
			itemMark () {
				return this.activeLength > 1 ? 'items' : 'item'
			}
		},
		methods: {
			pushState (e) {
        e.preventDefault()
        this.$emit('routeChange', e.target.pathname)
        window.history.pushState(
          null,
          {route: e.target.pathname},
          e.target.pathname
        )
			},
			deleteCompleted () {
				this.$emit('deleteCompleted')
			}
		}
  }
</script>

<style scoped>
.app-footer {
	color: #777;
	padding: 10px 15px;
	height: 40px;
	text-align: center;
	border-top: 1px solid #e6e6e6;
}

.app-footer:before {
	content: '';
	position: absolute;
	right: 0;
	bottom: 0;
	left: 0;
	height: 50px;
	overflow: hidden;
	z-index: 0;
	box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2),
	            0 8px 0 -3px #f6f6f6,
	            0 9px 1px -3px rgba(0, 0, 0, 0.2),
	            0 16px 0 -6px #f6f6f6,
	            0 17px 2px -6px rgba(0, 0, 0, 0.2);
}

.todo-count {
	position: absolute;
	left: 15px;
}

.todo-count strong {
	font-weight: 300;
}

.todo-filters {
	position: relative;
	display: inline-block;
	width: 200px;
	padding: 0;
	margin: 0 auto;
	list-style: none;
}

.todo-filters li {
	display: inline;
}

.todo-filters li a {
	color: inherit;
	margin: 3px;
	padding: 3px 7px;
	text-decoration: none;
	border: 1px solid transparent;
	border-radius: 3px;
	cursor: pointer;
}

.todo-filters li a.selected,
.todo-filters li a:hover {
	border-color: rgba(175, 47, 47, 0.1);
}

.todo-filters li a.selected {
	border-color: rgba(175, 47, 47, 0.2);
}

.clear-completed,
html .clear-completed:active {
	float: right;
	position: relative;
	line-height: 20px;
	text-decoration: none;
	cursor: pointer;
	position: relative;
}

.clear-completed:hover {
	text-decoration: underline;
}

.info {
	margin: 65px auto 0;
	color: #bfbfbf;
	font-size: 10px;
	text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
	text-align: center;
}

.info p {
	line-height: 1;
}

.info a {
	color: inherit;
	text-decoration: none;
	font-weight: 400;
}

.info a:hover {
	text-decoration: underline;
}

.todo-delete-completed {
	position: absolute;
  right: 15px;
  color: #aaa;
  font-size: 0.9em;
	cursor: pointer;
}
</style>
