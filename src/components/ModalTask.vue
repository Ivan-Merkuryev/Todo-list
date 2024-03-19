<template>
  <div class="modal">
    <p class="modal-edit">Edit task</p>
    <form class="modal-form" @submit.prevent="updateTask">
      <textarea type="text" v-model="editedText.text"></textarea>
      <div class="modal-btn">
        <button type="submit" class="modal-btn_save" @click="updateTask">
          <img src="./icons/save.svg" alt="" />
        </button>
        <button class="modal-btn_close" @click="closeModal">
          <img src="./icons/delete1.svg" alt="" />
        </button>
      </div>
    </form>
  </div>
  <div @click="closeModal" class="overlay"></div>
</template>

<script>
export default {
  props: {
    tasks: {
      type: Array,
    },
    currentTask: {
      type: Object,
    },
  },
  emits: ["updateTask", "closeModal"],
  data() {
    return {
      editedText: { text: this.currentTask.text },
    };
  },
  methods: {
    setCurrentTask() {
      this.editedText.text = this.currentTask.text;
    },
    updateTask() {
      if (
        this.editedText.text !== "" &&
        this.tasks.indexOf(this.editedText) === -1
      ) {
        const index = this.tasks.indexOf(this.currentTask);
        const updatedTask = { text: this.editedText.text };
        this.tasks.splice(index, 1, updatedTask);
        this.$emit("updateTask", updatedTask);
      }
    },
    editeTask() {
      if (this.editedTask !== "") {
        const updatedTask = { text: this.editedText };
        this.$emit("updateTask", updatedTask);
      }
    },
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style scoped lang="sass">
.modal
  position: fixed
  top: 50%
  left: 50%
  transform: translate(-50%, -50%)
  z-index: 2
  background-color: white
  border-radius: 10px
  padding: 10px 20px
  .modal-edit
    font-family: 'Indie Flower', cursive
.modal-form
  display: flex
  flex-direction: column

.modal textarea
    border-radius: 20px
    padding: 10px
    outline: none
    resize: none
    font-size: 16px
    height: 50px
    border: 1px #FFC5F6 solid
    scrollbar-width: none
    -ms-overflow-style: none
textarea::-webkit-scrollbar
  width: 0px
  background-color: transparent

.modal-btn
    margin-top: 10px
    display: flex
    justify-content: space-around
.modal-btn_save, .modal-btn_close
    background-color: inherit
    outline: none
    border: none
    cursor: pointer
.modal-icon
    width: 30px
    height: 30px

.overlay
    position: fixed
    top: 0
    left: 0
    width: 100%
    height: 100%
    background-color: rgba(0,0,0,0.5)
    z-index: 1
</style>
