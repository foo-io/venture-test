<template>
  <tr class="tbody__item">
    <td>
      <div class="product" @click.stop="showModal()">
        <div class="product__photo">
          <img :src="item.properties.products.photo" alt="">
        </div>
        <div class="block__info">
          <div class="block__value">
            {{ item.properties.products.value }}
          </div>
          <div class="block__description">
            {{ item.properties.products.description }}
          </div>
        </div>
      </div>
    </td>
    <td>
      <div class="block__info">
        <div class="block__value">
          {{ formatPrice(item.properties.earning.value) }}
        </div>
        <div class="block__description">
          {{ item.properties.earning.description }}
        </div>
      </div>
    </td>
    <td>
      <div class="block__info">
        <div class="block__value">
          {{ formatPrice(item.properties.comission.value) }}
        </div>
        <div class="block__description">
          {{ item.properties.comission.description }}
        </div>
      </div>
    </td>
    <td>
      <div class="block__info">
        <div class="block__value">
          {{ item.properties.company.value }}
        </div>
        <div class="block__description">
          {{ item.properties.company.description }}
        </div>
      </div>
    </td>
    <td>
      <div class="block__info">
        <div class="block__value">
          <star-rating 
            :increment="0.5"
            :star-size="20"
            :show-rating="false"
            :rating="item.properties.rating.value" 
            :read-only="true"
            :padding="0.5"
            :star-points="[23,2, 14,17, 0,19, 10,34, 7,50, 23,43, 38,50, 36,34, 46,19, 31,17]"
            active-color="#FFA800"
            inactive-color="#ffa8004d"
          ></star-rating>
        </div>
        <div class="block__description">
          {{ item.properties.rating.description }}
        </div>
      </div>
    </td>
    <td>
      <div class="item__actions">
        <button class="button button__color-green" @click.stop="toggleDropdown">View Offer</button>
        <div class="action__dropdown" v-if="isDropdownActive" v-click-outside="closeDropdown">
          <ul class="action__list">
            <li class="action__list-item">
              <button class="action__button" @click="duplicateTask()">
                <img src="/icons/duplicate.svg" alt="Duplicate">
                <span>Duplicate</span>
              </button>
            </li>
            <li class="action__list-item">
              <button class="action__button" @click="removeTask()">
                <img src="/icons/delete.svg" alt="Delete">
                <span>Delete Task</span>
              </button>
            </li>
          </ul>
        </div>
      </div>
    </td>
  </tr>
  <VModal :item="item" :isModalActive="isModalActive" @hide-modal="hideModal"></VModal>
</template>

<script>
import StarRating from 'vue-star-rating'
import VModal from './v-modal.vue'

export default {
  props: {
    item: Object
  },
  data() {
    return {
      isDropdownActive: null,
      isModalActive: false,
    }
  },
  emits: ['removeTask', 'duplicateTask'],
  methods: {
    removeTask: function () {
      this.$emit('removeTask', this.$props.item.id)
      this.closeDropdown()
    },
    duplicateTask: function () {
      this.$emit('duplicateTask', this.$props.item.id)
      this.closeDropdown()
    },
    closeDropdown: function () {
      this.isDropdownActive = false
    },
    toggleDropdown: function (idx) {
      this.isDropdownActive = !this.isDropdownActive
    },
    showModal: function() {
      this.isModalActive = true;
    },
    hideModal: function() {
      this.isModalActive = false;
    },
    formatPrice: function (value) {
      if (typeof value !== "number") return value;

      const formatter = new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
        minimumFractionDigits: 0
      });

      return formatter.format(value);
    }
  },
  components: {
    StarRating,
    VModal
},
  
}
</script>

<style>
.product {
  display: flex;
  align-items: center;
  cursor: pointer;
}
.product:hover .block__value {
  color: #3699FF;
}
.product .block__value {
  transition: color .2s ease;
}
.product__photo {
  background: #F3F6F9;
  border-radius: 6px;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  overflow: hidden;
  margin-right: 15px;
}
.product__photo img {
  width: 40px;
  height: 40px;
}
.block__value {
  font-weight: 600;
  font-size: 14px;
  line-height: 21px;
  color: #464E5F;
}
.block__description {
  font-weight: 500;
  font-size: 13px;
  line-height: 20px;
  color: #B5B5C3;
}
.item__actions {
  display: flex;
  justify-content: flex-end;
  position: relative;

}
.action__dropdown {
  position: absolute;
  min-width: 198px;
  z-index: 5;

  right: 0;
  top: calc(100% + 5px);
  
  padding: 8px;

  background: rgba(255, 255, 255, 0.92);
  border: 1px solid rgba(67, 73, 97, 0.1);
  box-shadow: 0px 5px 6px rgba(67, 73, 97, 0.08);
  border-radius: 12px;
}
.action__list {
  list-style: none;
  margin: 0;
  padding: 0;
}
.action__button {
  display: flex;
  border-radius: 9px;
  padding: 8px 12px;
  width: 100%;
  transition: background-color .2s ease;
  background: transparent;
}
.action__button:hover {
  background-color: rgba(67, 73, 97, 0.06);
}
.action__button img {
  width: 24px;
  margin-right: 16px;
}
.action__button span {
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #0F1014;
}

.item__actions .button {
  z-index: 4;
}
.tbody__item td {
  padding-top: 24px;
}
</style>