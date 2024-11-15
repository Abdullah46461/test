<template>
  <div class="container">
    <OrderFilter @filterOrders="fetchOrders" />
    <p>Найдено {{ orders.length }} заказов</p>
    <table class="order-table">
      <thead>
      <tr>
        <th>Номер заказа</th>
        <th>Дата-время создания</th>
        <th>Статус заказа</th>
        <th>Тип отгрузки</th>
        <th>Дата отгрузки</th>
        <th>Способ оплаты</th>
        <th>Место отгрузки</th>
        <th>ФИО клиента</th>
        <th>Сумма заказа</th>
        <th>Количество товаров</th>
        <th>Кто создал</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="order in orders" :key="order.id" @click="showOrderDetails(order)">
        <td>{{ order.id }}</td>
        <td>{{ order.created }}</td>
        <td :class="statusClass(order.status.id)">{{ order.status.title }}</td>
        <td>{{ order.shipment.method.title }}</td>
        <td>{{ order.shipment.date }}</td>
        <td>{{ order.payment.method.title }}</td>
        <td>{{ order.shop.title }}</td>
        <td>{{ order.client.firstname }} {{ order.client.lastname }}</td>
        <td>{{ order.amount }}</td>
        <td>{{ order.quantity }}</td>
        <td>{{ order.operator }}</td>
      </tr>
      </tbody>
    </table>
    <OrderModal v-if="showModal" :order="selectedOrder" @close="closeModal" />
  </div>
</template>

<script>
import OrderFilter from './components/OrderFilter.vue';
import OrderModal from './components/OrderModal.vue';

export default {
  components: {
    OrderFilter,
    OrderModal,
  },
  data() {
    return {
      orders: [],
      showModal: false,
      selectedOrder: null,
    };
  },
  mounted() {
    this.fetchOrders();
  },
  methods: {
    async fetchOrders(filter = {}) {
      try {
        const params = new URLSearchParams();

        // Если есть фильтры, добавляем их в параметры запроса
        if (filter.orderId) {
          // Если есть orderId, ищем заказ по ID
          const res = await fetch(`https://msk.ivanor.ru/front/service/test-api/v1/order/${filter.orderId}`, {
            method: 'GET',
            headers: {
              'Token': '345d11ebb939fccac50b55ffb6a4c43a',  // Убедитесь, что используете правильный токен
              'Content-Type': 'application/json',
            },
          });
          const data = await res.json();
          this.orders = [data.order];  // Если заказ найден, добавляем его в список
        } else {
          // Если нет фильтра по ID, получаем все заказы по датам и статусам
          if (filter.dateFrom) params.append('dateFrom', filter.dateFrom);
          if (filter.dateTo) params.append('dateTo', filter.dateTo);
          if (filter.status) params.append('orderStatus', filter.status);

          const res = await fetch(`https://msk.ivanor.ru/front/service/test-api/v1/orders?${params.toString()}`, {
            method: 'GET',
            headers: {
              'Token': '345d11ebb939fccac50b55ffb6a4c43a',  // Убедитесь, что используете правильный токен
              'Content-Type': 'application/json',
            },
          });

          const data = await res.json();
          this.orders = data.orders;  // Если фильтры, то обновляем весь список заказов
        }
      } catch (error) {
        console.error("Failed to fetch orders:", error);
      }
    },

    statusClass(statusId) {
      return {
        'status-completed': statusId === 4,
        'status-reserved': statusId === 6,
        'status-cancelled': statusId === 5,
      };
    },
    showOrderDetails(order) {
      this.selectedOrder = order;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedOrder = null;
    },
  },
};
</script>

<style scoped>
.container {
  padding: 20px;
}
.order-table {
  width: 100%;
  border-collapse: collapse;
}
.order-table th, .order-table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}
.status-completed {
  background-color: #d4edda;
}
.status-reserved {
  background-color: #f8d7da;
}
.status-cancelled {
  background-color: #f8d7da;
}
</style>
