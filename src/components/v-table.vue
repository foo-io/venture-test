<template>
  <table class="table">
    <thead class="thead">
      <tr>
        <th class="active">Products</th>
        <th>Earnings</th>
        <th>Comission</th>
        <th>Company</th>
        <th>Rating</th>
        <th></th>
      </tr>
    </thead>

    <tbody class="tbody">
      <VItem 
        v-for="item in users"
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
    }
  },
  async mounted() {
    const res = await fetch('/api/users.json')
    const json = await res.json();
    this.users = json
  },
  methods: {
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
