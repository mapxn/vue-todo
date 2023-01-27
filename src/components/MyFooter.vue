<template>
  <div class="todo-footer" v-show="totalTodos">
    <label>
      <input type="checkbox" :checked="isAll" @change="selectAll"/>
    </label>
    <span>
          <span>已完成{{ doneCnt }}</span> / 全部{{ totalTodos }}
        </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ['todos'],
  computed: {
    doneCnt() {
      return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0)
    },
    totalTodos() {
      return this.todos.length
    },
    isAll: {
      get() {
        return this.doneCnt === this.totalTodos && this.doneCnt > 0
      },
      set(value) {
        // this.checkAllTodo(value)
        this.$emit('checkAllTodo', value)
      }
    }
  },
  methods: {
    selectAll(e) {
      this.todos.forEach(todo => {
        if (todo.done !== e.target.checked)
          this.$emit('checkAllTodo', todo.id)
      })
    },
    clearAll() {
      this.$emit('clearAllTodo')
    }
  }
}
</script>

<style scoped>

/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}

</style>