<template>
  <div class="todo-list">
    <div class="first-block"></div>
    <div class="second-block"></div>
    <div class="card">
      <div class="header">
        <h1>Todo list</h1>
        <button>Add</button>
      </div>
      <div>
        <transition-group name="fade" tag="ul" class="tasks">
          <task-item v-for="task in tasks"
                     @complete="completeTask(task)"
                     :task="task"
                     :key="task.id"></task-item>
        </transition-group>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios'
import TaskItem from './TaskItem'
export default {
  name: 'TodoList',
  data() {
    return {
      tasks: null,
      loading: true,
      errored: false
    }
  },
  components: {
    TaskItem
  },
  mounted() {
    axios
        .get('https://jsonplaceholder.typicode.com/todos')
        .then(response => {
          this.tasks = response.data;
        })
        .catch(error => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
  },
  methods: {
    completeTask(task) {
      task.completed = !task.completed;
    }
  }
}
</script>

<style scoped lang="scss">
  .todo-list {
    position: relative;
    .first-block {
      position: absolute;
      top: -20px;
      left: 1px;
      z-index: 40;
      width: 395px;
      height: 68px;
      background: #31394D;
      border-radius: 20px;
      transform: rotate(-2deg);
    }
    .second-block {
      position: absolute;
      top: -31px;
      left: 3px;
      z-index: 30;
      width: 341px;
      height: 85px;
      border-radius: 20px;
      background: #4F5565;
      transform: rotate(-4deg);
    }
    .card {
      width: 420px;
      height: auto;
      border-radius: 20px;
      background: #252E42;
      padding: 30px;
      position: relative;
      z-index: 50;
      .header {
        display: flex;
        justify-content: space-between;
        margin-bottom: 40px;
        h1 {
          margin: 0;
          font-weight: bold;
          color: #fff;
          font-size: 28px;
        }
        button {
          background: #FF8469;
          font-weight: 500;
          border: none;
          outline: none;
          color: #fff;
          padding: 5px 7px;
          border-radius: 7px;
          cursor: pointer;
          text-transform: uppercase;
        }
      }
      .tasks-item {
        display: flex;
        margin-bottom: 30px;
      }
    }
  }

</style>
