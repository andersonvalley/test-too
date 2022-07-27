<script>
import './header.scss'

export default {
  data() {
    return {
      c: '',
    }
  },

  methods: {
    showModal() {
      this.$emit('modal')
    },
  },
}
</script>

<template>
  <header class="header">
    <h1 class="header__title">To do list</h1>
    <button @click="showModal()" class="btn-reset btn-add">
      <img src="@/assets/img/plus.svg" alt="add-btn" />
    </button>
  </header>
</template>
