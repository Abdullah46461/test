<template>
  <div >
    <div >
      <input v-model="filter.orderId" type="text" placeholder="Номер заказа" />
      <button @click="searchById">Искать</button>
    </div>
    <div>
      <input v-model="filter.dateFrom" type="date" placeholder="Дата с" />
      <input v-model="filter.dateTo" type="date" placeholder="Дата по" />
      <select v-model="filter.status" >
        <option value="4">Заказ выполнен</option>
        <option value="5">Заказ отменен</option>
        <option value="6">Заказ зарезервирован</option>
      </select>
      <button @click="searchByFilter">Искать</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filter: {
        orderId: '',
        dateFrom: '',
        dateTo: '',
        status: [],
      },
    };
  },
  methods: {
    searchById() {
      this.$emit('filterOrders', { orderId: this.filter.orderId });
    },
    searchByFilter() {
      const { dateFrom, dateTo, status } = this.filter;
      this.$emit('filterOrders', { dateFrom, dateTo, status: status.join(',') });
    },
  },
};
</script>
