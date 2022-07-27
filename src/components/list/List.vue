<script>
import './list.scss'

export default {
  props: {
    items: {
      type: Array,
    },
  },

  methods: {},
}
</script>

<template>
  <div class="list">
    <ul class="list__head row">
      <li class="list__head-item"></li>
      <li class="list__head-item row-head item__description description">Описание</li>
      <li class="list__head-item row-head item__status">Статус</li>
      <li class="list__head-item row-head">Дата</li>
    </ul>
    <div class="done" v-if="items.length === 0">Ничего нет</div>
    <ul class="list__body">
      <li class="item row" v-for="item in items" :key="item.id" @click="$emit('compleated', item)">
        <div class="item__compleated">
          <span class="item__compleated-icon">
            <img v-if="item.compleated" src="@/assets/img/compleated.svg" alt="compleated" />
          </span>
        </div>
        <p class="item__description">{{ item.text }}</p>
        <span :class="item.compleated ? 'item__status item__status-compleated' : 'item__status'">{{
          item.compleated ? 'Выполнено' : 'В работе'
        }}</span>
        <span class="item__data">{{ item.data }}</span>
      </li>
    </ul>
  </div>
</template>
