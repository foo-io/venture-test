<template>
  <table class="table">
    <thead class="thead">
      <tr>
        <th @click="sort('products')" :class="{active: sortName === 'products'}">
          Products
          <span v-if="sortName === 'products'">{{ sortDir === 'asc' ? '&darr;' : '&uarr;' }}</span>
        </th>
        <th @click="sort('earning')" :class="{active: sortName === 'earning'}">
          Earnings
          <span v-if="sortName === 'earning'">{{ sortDir === 'asc' ? '&darr;' : '&uarr;' }}</span>
        </th>
        <th @click="sort('comission')" :class="{active: sortName === 'comission'}">
          Comission
          <span v-if="sortName === 'comission'">{{ sortDir === 'asc' ? '&darr;' : '&uarr;' }}</span>
        </th>
        <th @click="sort('company')" :class="{active: sortName === 'company'}">
          Company
          <span v-if="sortName === 'company'">{{ sortDir === 'asc' ? '&darr;' : '&uarr;' }}</span>
        </th>
        <th @click="sort('rating')" :class="{active: sortName === 'rating'}">
          Rating
          <span v-if="sortName === 'rating'">{{ sortDir === 'asc' ? '&darr;' : '&uarr;' }}</span>
        </th>
        <th></th>
      </tr>
    </thead>

    <tbody class="tbody">
      <VItem 
        v-for="item in sortedUsers"
        :key="item.id" 
        :item="item" 
        @remove-task="removeTask" 
        @duplicate-task="duplicateTask"
      ></VItem>
    </tbody>

  </table>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import VItem from './v-item.vue';

export default {
  data() {
    return {
      users: [],
      sortName: 'products',
      sortDir: 'asc'
    }
  },
  async mounted() {
    const res = await fetch('/api/users.json')
    const json = await res.json();
    this.users = json
  },
  computed: {
    sortedUsers: function() {
      return this.users.sort((a, b) => {
        let mod = 1;

        if (this.sortDir === 'desc') mod = -1;
        if (a.properties[this.sortName].value < b.properties[this.sortName].value) return -1 * mod;
        if (a.properties[this.sortName].value > b.properties[this.sortName].value) return 1 * mod;
        
        return 0;
      });
    }
  },
  methods: {
    sort: function (param) {
      if (param === this.sortName) {
        this.sortDir = this.sortDir === 'asc' ? 'desc' : 'asc'
      }
      this.sortName = param;
    },
    duplicateTask: function (idx) {
      let duplicateId = this.users.findIndex(x => x.id === idx);
      let duplicateItem = {}

      Object.assign(duplicateItem, this.users[duplicateId])

      duplicateItem.id = uuidv4()
      this.users.push(duplicateItem)
    },
    removeTask: function (idx) {
      let removeId = this.users.findIndex(x => x.id === idx);
      this.users.splice(removeId, 1)
    },
    
  },
  components: {
    VItem
  }
}
</script>

<style scoped>
.table {
  width: 100%;
  border-collapse: collapse;
}
.thead {
  text-align: left;
  background: #f3f6f9;
  border-radius: 6px;
}
th {
  padding: 12px 0;
  text-transform: uppercase;
  font-weight: 600;
  font-size: 12px;
  line-height: 18px;
  letter-spacing: 0.03em;
  color: #B5B5C3;
  cursor: pointer;
}
th.active {
  color: #464E5F;
}
th:first-child {
  padding-left: 20px;
  border-radius: 6px 0 0 6px;
}
th:last-child {
  border-radius: 0 6px 6px 0;
}



</style>
