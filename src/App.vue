<script setup>
import Header from './components/header/Header.vue'
import Filter from './components/filter/Filter.vue'
import List from './components/list/List.vue'
import Modal from './components/modal/Modal.vue'
</script>

<template>
  <main ref="main">
    <Header @modal="showModal"></Header>
    <Filter @sort="sort" @search="search"></Filter>
    <List @compleated="compleated" :items="sortedAndSearched"></List>
  </main>
  <Modal @closeModal="showModal" v-if="visiblelModal" @addItem="addItem"></Modal>
</template>

<script>
export default {
  data() {
    return {
      items: JSON.parse(localStorage.getItem('todos')) || [],
      searched: [],
      visiblelModal: false,
      searchValue: '',
      sortedValue: 'Дата',
    }
  },
  methods: {
    addItem(item) {
      this.items.push(item)
      this.visiblelModal = false
      this.setLocalStorage()
      this.$refs.main.style.filter = ''
    },

    compleated(todo) {
      const item = this.items.find(item => item.id === todo.id)
      item.compleated = !item.compleated
      this.setLocalStorage()
    },

    showModal() {
      this.visiblelModal = !this.visiblelModal
      if (!this.visiblelModal) {
        this.$refs.main.style.filter = ''
        return
      }

      this.$refs.main.style.filter = 'blur(4px)'
    },

    setLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.items))
    },

    search(search) {
      this.searchValue = search.toLowerCase()
    },

    sort(name) {
      this.sortedValue = name
    },
  },

  computed: {
    sorted() {
      const sortedBy = ['Статус', 'Дата']

      if (this.sortedValue === sortedBy[0]) {
        return [...this.items].sort((a, b) => a.compleated - b.compleated)
      }

      if (this.sortedValue === sortedBy[1]) {
        const replace = data => data.split('.').join('/')
        return [...this.items].sort((a, b) => new Date(replace(b.data)) + new Date(replace(a.data)))
      }

      return [...this.items]
    },

    sortedAndSearched() {
      const searchByComplited = [
        { name: 'выполнено', flag: true },
        { name: 'в работе', flag: false },
      ]

      const i = searchByComplited.findIndex(item => item.name === this.searchValue)
      const index = i !== -1 ? i : 0

      if (this.searchValue === searchByComplited[index].name) {
        return this.sorted.filter(item => item.compleated === searchByComplited[index].flag)
      }

      return this.sorted.filter(
        item =>
          item.data.toLowerCase().includes(this.searchValue) ||
          item.text.toLowerCase().includes(this.searchValue) ||
          String(item.id).toLowerCase().includes(this.searchValue)
      )
    },
  },

  mounted() {
    this.$refs.main
  },
}
</script>
