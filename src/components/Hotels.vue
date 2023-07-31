<script setup>
import Stars from "./Star.vue";
defineProps({
  hotel: {},
  cheapest: null,
});
</script>

<template>
  <div class="card d-grid" :class="{ selected: cheapest }">
    <h1 class="">{{ hotel.name }} Hotel</h1>

    <Stars :rate="hotel.rating" :code="hotel.code" />
    <div class="d-flex">
      <div class="w-50">
        <h4 class="bold">Reward Customer</h4>
        <p>
          Weekday:
          <span class="price"
            >${{ hotel.prices.reward_customer.weekday_price }}</span
          >
        </p>
        <p>
          Weekend:
          <span class="price"
            >${{ hotel.prices.reward_customer.weekend_price }}</span
          >
        </p>
      </div>
      <div class="w-50 separator">
        <h4 class="bold">Regular Customer</h4>
        <p>
          Weekday:
          <span class="price"
            >${{ hotel.prices.regular_customer.weekday_price }}</span
          >
        </p>
        <p>
          Weekend:
          <span class="price"
            >${{ hotel.prices.regular_customer.weekend_price }}</span
          >
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  margin: 0 10px 15px 10px;
  padding: 10px 20px;
  border: 1px solid #019df433;
  background-color: #e5f5fd;
  border-radius: 16px;
  width: 30%;
}
.price {
  color: #0b2739;
  font-weight: 500;
  font-size: 14px;
}

.selected {
  border: 2px solid #bde4a4;
  background: #edffde;
}

.separator {
  border-left: 1px solid #019df433 !important;
  padding-left: 20px;
}

.selected .separator {
  border-left: 1px solid #bde4a4 !important;
}
@media (max-width: 480px) {
  .card {
    width: 100% !important;
  }
}
</style>
