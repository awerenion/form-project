<template>
  <div>
    <div
      class="container"
      :class="getBg"
    >
      <button
        class="basic-button"
        @click="showModal"
      >
        Form
      </button>
    </div>
    <basic-form
      v-if="isShowModal"
      class="form"
      @closeModal="closeModal"
    />
  </div>
</template>

<script>
import BasicForm from '@/components/BasicForm'

export default {
  name: 'MainPage',
  components: {
    BasicForm
  },
  data () {
    return {
      isShowModal: false
    }
  },
  computed: {
    getBg () {
      return {
        default: !this.isShowModal,
        modal: this.isShowModal
      }
    }
  },
  methods: {
    showModal () {
      this.isShowModal = true
    },
    closeModal () {
      this.isShowModal = false
    }
  }
}
</script>

<style scoped>
  .container {
    background: #F0ECE0;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
  }

  .basic-button {
    border: 2px solid teal;
    border-radius: 2px;
    width: 140px;
    height: 80px;
    background: inherit;
    font-size: 16px;
    letter-spacing: 0.1rem;
    cursor: pointer;
  }

  .form {
    z-index: 3;
  }

  .default {
    background:  #F0ECE0;
  }

  .modal {
    background: #808080;
  }
</style>
