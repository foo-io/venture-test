<template>
  <div class="modal" v-if="isModalActive" v-click-outside="hideModal">
    <h2>Я всплывающее окно =)</h2>
    <p>Вот данные, которое принесло НЛО:</p>
    <pre>
{{ item }}
    </pre>
    <div class="modal__actions">
      <button class="button button__color-red" @click="hideModal">
        Close modal
      </button>
    </div>
  </div>
  <div class="overflow-mask" v-if="isModalActive"></div>
</template>

<script>
export default {
  props: {
    item: Object,
    isModalActive: Boolean
  },
  methods: {
    hideModal: function() {
      this.$emit('hideModal');
      this.isActive = false;
    },
  },
  emits: ['hideModal']
}
</script>

<style>
.overflow-mask {
  position: fixed;
  z-index: 9;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.6);
}
.modal {
  padding: 2rem;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  width: 550px;
  height: 400px;
  overflow-y: auto;
  z-index: 10;
  background: rgba(255, 255, 255, 0.92);
  border: 1px solid rgba(67, 73, 97, 0.1);
  box-shadow: 0px 5px 6px rgba(67, 73, 97, 0.08);
  border-radius: 12px;
}
.modal__actions {
  display: flex;
  justify-content: flex-end;
}
</style>