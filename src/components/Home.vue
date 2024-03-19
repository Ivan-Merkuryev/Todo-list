<template>
  <div class="app">
    <h1>To-Do List</h1>
    <div class="form-tasks">
      <form method="get" class="form" @submit.prevent="addTask">
        <textarea placeholder="Add task..." v-model="taskText"></textarea>
        <button class="form-button" @click="addTask()" type="submit">
          <img class="form-button_icon" src="./icons/submit.svg" alt="" />
        </button>
      </form>
      <tasks-list
        :tasks="tasks"
        @filterAll="updateImages(0)"
        @filterCompleted="updateImages(1)"
        @filterIncompleted="updateImages(2)"
      />
    </div>

    <img class="img-list" src="./icons/list.svg" alt="list" />
    <img
      v-if="img === 0"
      class="img-panther"
      src="./icons/panther.svg"
      alt="panther"
    />
    <img v-if="img === 1" class="img-panther" src="./icons/chill.svg" alt="" />
    <img v-if="img === 2" class="img-panther" src="./icons/sad.svg" alt="" />
  </div>
</template>

<script>
import TasksList from "./TasksList.vue";
export default {
  components: { TasksList },
  data() {
    return {
      taskText: "",
      tasks: [],
      img: 0
    };
  },

  async mounted() {
    const data = await localStorage.getItem("tasks");
    if (data) {
      this.tasks = JSON.parse(data);
    }
  },
  watch: {
    tasks: {
      handler() {
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      },
      deep: true,
    },
  },
  methods: {
    addTask() {
      const task = {
        text: this.taskText,
        id: Date.now(),
        completed: false,
      };
      if (this.taskText.trim()) {
        this.tasks.unshift(task);
        this.taskText = "";
      }
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    updateImages(img) {
      this.img = img;
    }
  },
};
</script>

<style lang="sass" scoped>
.app
  height: 100%
  width: 96%
  margin: 0 auto
  @media screen and (min-width: 1200px)
    max-width: 1200px
    margin: 0 auto

h1
  text-align: center
  font-family: 'Indie Flower', cursive
  margin: 0

.form
  position: relative
  width: 350px
  height: 100px
  margin-top: 30px
  border-radius: 20px
  display: flex
  @media screen and (max-width: 390px)
    width: 330px
  @media screen and (max-width: 360px)
    width: 310px
  @media screen and (max-width: 340px)
    width: 295px
  textarea
    border-radius: 20px
    padding: 15px
    width: 320px
    outline: none
    resize: none
    border: none
    font-size: 16px
    border: 1px #FFC5F6 solid
    @media screen and (max-width: 390px)
      width: 330px
    @media screen and (max-width: 360px)
      width: 310px
    @media screen and (max-width: 340px)
      width: 295px
  textarea:focus
    border: 1px #F997CF solid
.form-tasks
  width: 350px
  @media screen and (max-width: 390px)
    width: 330px
  @media screen and (max-width: 360px)
    width: 310px
  @media screen and (max-width: 340px)
    width: 295px

.form-button
  width: 30px
  height: 30px
  position: absolute
  bottom: 5px
  right: 5px
  background: none
  border: none
  outline: none
  cursor: pointer

  &_icon
    position: absolute
    top: 0
    left: 0
    width: 100%
    height: 100%

.img-list
  width: 270px
  top: 20%
  left: 40%
  z-index: -1
  position: fixed
  @media screen and (max-width: 400px)
    left: 37%
.img-panther
  width: 300px
  position: fixed
  bottom: 0
  left: 15%
  z-index: -1
</style>
