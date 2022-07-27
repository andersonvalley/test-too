<script>
import './filter.scss'

export default {
  data() {
    return {
      popupVisible: false,
      sortBy: [
        { name: 'Дата', id: 0 },
        { name: 'Статус', id: 1 },
      ],
      currentSortId: 0,
      searchvalue: '',
    }
  },

  methods: {
    showPopup() {
      this.popupVisible = !this.popupVisible
    },

    setSort(item) {
      this.currentSortId = item.id
      this.popupVisible = false

      this.$emit('sort', item.name)
    },

    inputText(e) {
      this.$emit('search', e.target.value)
    },
  },
}
</script>

<template>
  <div class="filter">
    <div class="search">
      <img class="search__icon" src="@/assets/img/search.svg" alt="search" />
      <input
        :value="searchvalue"
        @input="inputText"
        class="search__input"
        type="search"
        placeholder="Поиск ID, Имени, статуса или даты"
      />
    </div>
    <div class="sort">
      Сортировать по:
      <button @click="showPopup()" class="btn-reset sort__by">
        <span class="sort__by-text">{{ sortBy[currentSortId].name }}</span>
        <img src="@/assets/img/down.svg" alt="arrow" />
      </button>
      <div v-if="popupVisible" @click.stop="" class="sort__popup popup">
        <ul class="sort__popup popup__list" v-for="item in sortBy" :key="item.id">
          <li
            :class="currentSortId === item.id ? 'popup__item popup__item-active' : 'popup__item'"
            @click="setSort(item)"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
