<template>
  <div >
    <div >
      <input v-model="filter.orderId" type="text" placeholder="Номер заказа" />
      <button @click="searchById">Искать</button>
    </div>
    <div>
      <input v-model="filter.dateFrom" type="date" placeholder="Дата с" />
      <input v-model="filter.dateTo" type="date" placeholder="Дата по" />
      <multiselect
          v-model="filter.status"
          :options="statusOptions"
          :multiple="true"
          :close-on-select="false"
          placeholder="Выберите статус"
          track-by="value"
          label="title"
      ></multiselect>
      <button @click="searchByFilter">Искать</button>
    </div>
  </div>
</template>

<script>
  import Multiselect from 'vue-multiselect';
  import 'vue-multiselect/dist/vue-multiselect.min.css';

  export default {
  components: {Multiselect},
  data() {
  return {
  filter: {
  orderId: '',
  dateFrom: '',
  dateTo: '',
  status: [], // статус должен быть массивом, так как multiselect возвращает массив
},
  statusOptions: [
{value: '4', title: 'Заказ выполнен'},
{value: '5', title: 'Заказ отменен'},
{value: '6', title: 'Заказ зарезервирован'},
  ],
};
},
  methods: {
  searchById() {
  console.log('Ищем по ID:', this.filter.orderId);
  this.$emit('filterOrders', {orderId: this.filter.orderId});
},
  searchByFilter() {
  const {dateFrom, dateTo, status} = this.filter;

  const statusValues = status ? status.map(item => item.value).join(',') : '';

  this.$emit('filterOrders', {dateFrom, dateTo, status: statusValues});

  console.log('Фильтры отправлены:', {dateFrom, dateTo, status: statusValues});
},
},
};

</script>
