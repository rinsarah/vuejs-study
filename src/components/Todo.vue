<!-- Add "scoped" attribute to limit CSS to this component only-->
<style src="../styles/todo.css" ></style>

<template>
  <div>
    <!-- Add New Task Function -->
    <input
      type="text"
      class="textinput"
      placeholder="Insert new task"
      v-model="newList"
      @keyup.enter="addTodo"
    />
    <div class="add">
      <button class="addButton" @click="addTodo">ADD TASK</button>
    </div>

    <!-- Display Tasks -->
    <div v-for="(current, index) in tasksFilter" :key="current.id">
      <div class="item">
        <!-- Edit Task function -->
        <div v-if="!current.editing" @dblclick="editTodo(current)">
          {{ current.task }}
        </div>
        <input
          v-else
          type="text"
          class="textedit"
          v-model="current.task"
          @blur="editDone(current)"
          @keyup.enter="editDone(current)"
          v-focus
        />
        <div>
          <!-- edit using button
          <div v-if="!current.editing" class="edit" @click="editTodo(current)">
            &#128393;
          </div>
          <div v-else class="edit" @click="editDone(current)">&checkmark;</div> -->
          <!-- Delete Task function -->
          <div
            v-if="!current.editing"
            class="remove"
            @click="removeTodo(index)"
          >
            &times;
          </div>
        </div>
      </div>
      <div v-if="!current.complete & !current.editing" class="add">
        <button class="completeButton" @click="markComplete(current)">
          &checkmark; MARK AS COMPLETE
        </button>
      </div>
      <div v-else-if="!current.editing" class="completeLabel">Completed!</div>
      <!-- Filter Tasks -->
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>
        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
    </div>
    <button @click="displayDeleted">Deleted task</button>
    <div v-if="displayDelete">
      <div v-for="deltasks in deleted" :key="deltasks.id">
        {{ deltasks.task }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo List',
  data() {
    return {
      newList: '',
      newId: 4,
      filter: 'all',
      displayDelete: false,
      deleted: [],
      delId: 1,
      currentList: [
        {
          id: 1,
          task: 'Complete Research Code',
          complete: true,
          editing: false
        },
        {
          id: 2,
          task: 'Review Research Code',
          complete: false,
          editing: false
        },
        {
          id: 3,
          task: 'Pay Bills',
          complete: false,
          editing: false
        }
      ]
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newList.trim().length === 0) {
        alert('No task written, please check!')
        return
      }
      this.currentList.push({
        id: this.newId,
        task: this.newList,
        complete: false,
        editing: false
      })

      this.newList = ''
      this.newId++
    },
    removeTodo(index) {
      this.deleted.push({
        id: this.delId,
        task: this.currentList[index].task
      })

      this.currentList.splice(index, 1)

      this.delId++
      alert(this.delId)
    },
    editTodo(item) {
      item.editing = true
    },
    editDone(item) {
      if (item.task.trim().length === 0) {
        alert('No task written, please check!')
        return
      }
      item.editing = false
    },
    markComplete(item) {
      alert('Task completed!')
      item.complete = true
    },
    displayDeleted() {
      if (!this.displayDelete) this.displayDelete = true
      else this.displayDelete = false
      alert(this.displayDelete)
    }
  },
  computed: {
    tasksFilter() {
      if (this.filter === 'all') {
        return this.currentList
      } else if (this.filter === 'active') {
        return this.currentList.filter(list => !list.complete)
      } else if (this.filter === 'completed') {
        return this.currentList.filter(list => list.complete)
      }

      return this.currentList
    }
  }
}
</script>
