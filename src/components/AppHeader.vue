<template>
<header>
  <h1 class="app-header">{{ title }}</h1>
  <input
    class="new-todo"
    placeholder="What needs to be done?"
    @keyup.13="add"
  />
</header>
</template>

<script>
export default {
  name: 'app-header',
  data () {
    return {
      title: 'todos'
    }
  },
  methods: {
    add (e) {
      let tg = e.target
      if (tg.value.length) {
        this.$emit('addTodo', tg.value)
        tg.value = ''
      }
    }
  }
}
</script>

<style>
.app-header {
  position: absolute;
  top: -139px;
  width: 100%;
  font-size: 100px;
  font-weight: 100;
  text-align: center;
  color: rgba(175, 47, 47, 0.15);
  -webkit-text-rendering: optimizeLegibility;
  -moz-text-rendering: optimizeLegibility;
  text-rendering: optimizeLegibility;
}

.new-todo {
  padding: 16px 16px 16px 60px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
}
</style>
