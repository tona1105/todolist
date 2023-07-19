<template>
  <div>
    <div class="w-75 m-auto position-relative bg-todo" style="min-height: 500px;"
      :style="{ opacity: openModal === true ? 0.5 : 1 }">
      <span class="text-center fs-2 d-block mb-3 font-weight-bold" style="font-size: 2rem;">TODO LIST</span>
      <div class="d-flex justify-content-around mb-5 font-weight-bold text-white">
        <span class="bg-primary p-2 ">TOTAL: {{ total }}</span>
        <span class="bg-success p-2 ">SUCCESS: {{ success }}</span>
        <span class="bg-danger p-2 ">PENDING: {{ pending }}</span>
      </div>
      <!-- input new To do -->
      <form action="" class="text-center mb-3">
        <input v-model="newContent" type="text" placeholder="Enter new To-do" class="w-75" />
        <label for="" class="mx-2">Priority:</label>
        <select name="" id="" v-model="priority">
          <option value="High">High</option>
          <option value="Medium">Medium</option>
          <option value="Low">Low</option>
        </select>
        <i class="fa fa-arrow-right ml-1" @click="addNewToDo(newContent)" style="font-size: 1.5rem;"></i>
      </form>
      <!-- List Todo -->
      <div class="" style="font-size: 1.2rem;">
        <div class="row pl-5 " v-for="(item, index) in reversedList" :key="index" :class="setDoneTask(item)">
          <span class="col-6 todo-content" @click="detailTodo(item)">{{ item.content }}</span>
          <div class="col-6">
            <div class="row">
              <span class="col-6">
                <i class="fa fa-circle" aria-hidden="true" :class="classByPriority(item.priority)"></i>
                Priority: {{ item.priority }}
              </span>
              <span class="col-3">{{ item.Date }}</span>
              <div class="col-3">
                <input type="checkbox" class="mr-2 ml-2" v-model="item.isChecked" @click="doneTask(item)" :disabled="item.isDisabled">
                <i class="fa fa-trash" aria-hidden="true" @click="deleteTodo(item)"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Clear all -->
      <span @click="clearAll" class="position-absolute bg-dark text-light font-weight-bold p-2 btn"
        style="bottom: 0; font-size: 1.2rem;">Clear all</span>

    </div>
    <!-- Modal -->
    <div v-show="openModal === true" class="modal-todo w-75 m-auto">
      <div class="w-100">
        <input v-model="changeContent" type="text" placeholder="Enter new To-do" class="w-75" />
        <label for="" class="ml-4 mr-2">Priority:</label>
        <select name="" id="" v-model="changePriority" c>
          <option value="High">High</option>
          <option value="Medium">Medium</option>
          <option value="Low">Low</option>
        </select>
      </div>
      <div class="text-left">
        <div class="btn btn-primary ml-1" @click="changeTodo(itemOpen)">Change</div>
        <div class="btn btn-secondary mr-1" @click="cancelChange">Cancel</div>
      </div>
    </div>
  </div>
</template>

<script>
import 'font-awesome/css/font-awesome.css';
export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      listToDo: [],
      newContent: '',
      changeContent: '',
      priority: '',
      changePriority: '',
      currentDate: '',
      counter: 0,
      total: 0,
      success: 0,
      pending: 0,
      openModal: false,
      itemOpen: ''
    }
  },
  computed: {
    reversedList() {
      return this.listToDo.slice().reverse();
    },
  },
  mounted() {
    this.getCurrentDate();
  },
  methods: {
    addNewToDo(content) {
      if (this.newContent === '') alert('Write down the new content')
      else if (this.priority === '') alert('Pick the priority')
      else {
        console.log(this.counter);
        this.listToDo.push({
          id: this.counter,
          content: content,
          priority: this.priority,
          Date: this.currentDate,
          active: '',
          isChecked: false,
          isDisabled: false
        })
        this.newContent = ''
        this.counter++
        this.total++
        this.pending++
      }

    },
    getCurrentDate() {
      const options = { month: 'long', day: 'numeric' };
      this.currentDate = new Date().toLocaleDateString(undefined, options);
    },
    classByPriority(priority) {
      if (priority === 'High') return 'high'
      else if (priority === 'Medium') return 'medium'
      else return 'low'
    },
    detailTodo(item) {
      const index = this.listToDo.indexOf(item);
      this.changeContent = this.listToDo[index].content
      this.changePriority = this.listToDo[index].priority
      this.openModal = true
      this.itemOpen = item
      console.log(this.openModal);
    },
    changeTodo(item) {
      const index = this.listToDo.indexOf(item);
      this.listToDo[index].content = this.changeContent
      this.listToDo[index].priority = this.changePriority
      this.openModal = false
    },
    cancelChange() {
      this.openModal = false

    },
    doneTask(item) {
      const index = this.listToDo.indexOf(item);
      console.log(index);
      if (this.listToDo[index]) {
        this.listToDo[index].active = true
        this.listToDo[index].isDisabled = true
        this.pending--
        this.success++
      }
    },
    setDoneTask(item) {
      const index = this.listToDo.indexOf(item);
      if (this.listToDo[index] && this.listToDo[index].active === true) return 'text-decoration-line-through'
      else return ''
    },
    deleteTodo(item) {
      const index = this.listToDo.indexOf(item);
      this.listToDo.splice(index, 1);
      console.log(index);
      this.total--
    },
    clearAll() {
      this.listToDo = []
      this.total = 0
    }
  }
}
</script>

<style>
.bg-todo {
  background: linear-gradient(to right, #86c4f9 8%, #59a1ff 79%, #05abe0 100%)
}

.modal-todo {
  min-height: 200px;
  background-color: #59a1ff;
  display: block;
  position: fixed;
  z-index: 2;
  top: 25%;
  right: 0;
  left: 0;
  padding: 3rem;
}

.high {
  color: red
}

.medium {
  color: yellow
}

.low {
  color: darkgrey
}

.text-decoration-line-through {
  text-decoration: line-through;
}
</style>
