<template>
  <header class="header">
    <div class="header__title">
      <h1>
        New Arrivals
      </h1>
      <p>
        More than 400+ new members
      </p>
    </div>
    <div class="header__action">
      <ul class="action__list">
        <li class="action__list-item">
          <button class="button button__color-blue">New Report</button>
        </li>
        <li>
          <button class="button button__color-red">Create</button>
        </li>
      </ul>
    </div>
  </header>
  <main class="main">
    <VTable></VTable>
  </main>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}
.header__title h1 {
  font-weight: 500;
  font-size: 18px;
  line-height: 21px;
  color: #212121;
  margin: 0;
}
.header__title p {
  font-weight: 500;
  font-size: 12px;
  line-height: 21px;
  color: #B5B5C3;
  margin-top: 7px;
  margin-bottom: 0;
}
.action__list {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}
.action__list-item {
  margin-right: 10px;
}
.action__list-item:last-child {
  margin-right: 0;
}
</style>

<script setup>
import VTable from "@/components/v-table.vue"
</script>
