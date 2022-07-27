<script>
import './modal.scss'

export default {
  data() {
    return {
      text: '',
    }
  },

  methods: {
    inputText(e) {
      this.text = e.target.value
    },

    addItem() {
      var dt = new Date()

      const newTodo = {
        id: dt.getMilliseconds(),
        data: dt.getDate() + '.' + ('0' + dt.getMonth()).slice(-2) + '.' + dt.getFullYear(),
        compleated: false,
        text: this.text,
      }

      this.$emit('addItem', newTodo)
      this.text = ''
    },
  },
}
</script>

<template>
  <div class="overflow" @click="$emit('closeModal')">
    <div @click.stop="" class="modal">
      <div class="modal__head">
        <div class="modal__title">Создать новую задачу</div>
        <div @click="$emit('closeModal')" class="modal__close">
          <img src="@/assets/img/close.svg" alt="close" />
        </div>
      </div>

      <div class="modal__group">
        <label class="modal__label" for="description">Описание</label>
        <input
          v-on:keyup.enter="addItem()"
          :value="text"
          @input="inputText"
          class="modal__input"
          type="text"
          id="description"
        />
      </div>

      <div class="modal__btn">
        <button v-on:keyup.enter="addItem()" @click="addItem()" class="btn-reset btn-create">Создать</button>
      </div>
    </div>
  </div>
</template>
