<template>
  <div class="modal-overlay" @click.self="close">
    <div class="modal-content">
      <div class="modal-header">
        <h3>Детали заказа #{{ order.id }}</h3>
        <button class="modal-close" @click="close" aria-label="Close">X</button>
      </div>

      <div class="modal-section">
        <h4>Информация о заказе</h4>
        <p>ID в системе: {{ order.uid1c || 'Не указано' }}</p>
        <p>Дата создания: {{ order.created || 'Не указано' }}</p>
        <p>Статус: {{ order.status?.title || 'Неизвестен' }}</p>
      </div>

      <div class="modal-section">
        <h4>Информация о клиенте</h4>
        <p>ФИО: {{ order.client?.firstname || 'Не указано' }} {{ order.client?.lastname || '' }}</p>
        <p>Телефон: {{ order.client?.phone || 'Не указано' }}</p>
      </div>

      <div class="modal-section">
        <h4>Информация о магазине</h4>
        <p>Магазин: {{ order.shop?.title || 'Не указано' }}</p>
      </div>

      <div class="modal-section">
        <h4>Оплата</h4>
        <p>Способ оплаты: {{ order.payment?.method?.title || 'Не указано' }}</p>
        <p v-if="order.payment?.status">Статус оплаты: {{ order.payment.status.title || 'Не указано' }}</p>
      </div>

      <div class="modal-section">
        <h4>Состав заказа</h4>
        <ul>
          <li v-for="item in order.items || []" :key="item.id">
            {{ item.title || 'Товар неизвестен' }} - {{ item.quantity || 0 }} шт. по {{ item.price || 0 }} ₽, {{ item.status || 'Не указан' }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    order: {
      type: Object,
      required: true,
    },
  },
  methods: {
    close() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 600px;
  width: 100%;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-close {
  background: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

.modal-section {
  margin-top: 10px;
}
</style>
