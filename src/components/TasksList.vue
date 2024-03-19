<template>
  <div class="popup">
    <img src="./icons/caret.svg" @click="openPopup()" />
    <p>
      show
      <span class="popup-select" @click="openPopup()">{{ filter }}</span>
    </p>
  </div>
  <ul class="filter" v-if="showPopup">
    <li class="filter-btn" :class="{'active': filter === 'all'}"  @click="showAllTasks">
      <button @click="openPopup()">all</button>
    </li>
    <li class="filter-btn" :class="{'active': filter === 'completed'}" @click="showCompletedTasks">
      <button @click="openPopup()">completed</button>
    </li>
    <li class="filter-btn" :class="{'active': filter === 'incompleted'}" @click="showIncompletedTasks">
      <button @click="openPopup()">incompleted</button>
    </li>
  </ul>
  <ul class="list">
    <li class="task" v-for="(task, index) in filteredTasks" :key="index">
      <span class="task-text" :class="{ completed: task.completed }"
        >- {{ task.text }}
      </span>
      <input type="checkbox" v-model="task.completed" />
      <button class="list-btn" @click="openModal(index)">
        <img src="./icons/edit.svg" alt="" />
      </button>
      <button class="list-btn" @click="removeTask(index)">
        <img src="./icons/delete1.svg" alt="" />
      </button>
    </li>
  </ul>
  <modal-task
    v-if="showModal"
    :tasks="tasks"
    :currentTask="currentTask"
    @updateTask="updateTask"
    @closeModal="closeModal"
  />
</template>

<script>
import ModalTask from "./ModalTask.vue";
export default {
  components: { ModalTask },
  props: {
    tasks: {
      type: Array,
      required: true,
    },
  },
  emits: ["filterAll", "filterCompleted", "filterIncompleted"],
  data() {
    return {
      showModal: false,
      showPopup: false,
      currentTask: {},
      filter: "all",
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === "completed") {
        return this.tasks.filter((task) => task.completed);
      } else if (this.filter === "incompleted") {
        return this.tasks.filter((task) => !task.completed);
      } else return this.tasks;
    },
  },
  methods: {
    openPopup() {
      this.showPopup = !this.showPopup;
    },
    openModal(index) {
      this.currentTask = this.tasks[index];
      if (!this.currentTask.completed) {
        this.showModal = true;
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    updateTask(updatedTask) {
      this.tasks.findIndex((task) => task.id === updatedTask.id);
      this.closeModal();
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    closeModal() {
      this.showModal = false;
    },
    showAllTasks() {
      this.filter = "all";
      this.$emit("filterAll");
    },
    showCompletedTasks() {
      this.filter = "completed";
      this.$emit("filterCompleted");
    },
    showIncompletedTasks() {
      this.filter = "incompleted";
      this.$emit("filterIncompleted");
    },
  },
};
</script>

<style lang="sass" scoped>
.task
  margin-top: 1em
  display: grid
  grid-template-columns: 250px 20px 20px 20px
  grid-auto-rows: min-content
  column-gap: 10px
  background-color: rgba(255,255,255, 0.3)
  @media screen and (max-width: 390px)
      grid-template-columns: 230px 20px 20px 20px
  @media screen and (max-width: 360px)
      grid-template-columns: 210px 20px 20px 20px
  @media screen and (max-width: 340px)
      grid-template-columns: 195px 20px 20px 20px

.task-text
  margin-right: 0.75em
.completed
  opacity: 0.5

.popup
  display: flex
  align-items: center
  margin-top: 10px
  img
    cursor: pointer
  p
    margin: 0
.popup-select
  cursor: pointer


.filter
  position: absolute
  z-index: 1
  background-color: white
  border-radius: 10px
  border: 1px solid #F997CF
  box-shadow: 2px 2px 4px 0px rgba(0, 0, 0, .2)
  overflow: hidden
  .filter-btn
    padding: .5em
  .filter-btn:hover
    background-color: #FFEDF6
  button
    border: 0
    background: transparent
    font-size: 16px
    width: 100%
    display: flex
  .active 
    background-color: #FFDFF6

.list-btn
  background-color: inherit
  outline: none
  border: none
  cursor: pointer
  margin-right: 0.5em
  padding: 0

.list
  height: 340px
  overflow-y: scroll
  -ms-overflow-style: none
  scrollbar-width: none
.list::-webkit-scrollbar
  width: 0px
  background-color: transparent

input
    width: 18px
    margin: 0
</style>
