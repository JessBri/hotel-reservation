<script setup>
import { ref } from "vue";
import json from "./data/hotels.json";
import ListHotels from "./components/Hotels.vue";
import VueDatePicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

const listHotels = json;
const startDate = ref(new Date(2009, 2));
const date = ref();
const customerType = ref(false);
var errorDate = ref(false);
var showHotel = ref(false);
var lowestHotel = {};

//Validate date
function validar() {
  console.log(customerType.value, date.value);
  if (date.value == undefined) {
    errorDate.value = true;
  } else {
    showHotel.value = false;
    errorDate.value = false;
    lowestHotel = {};
    sumPrices();
  }
}

//calculate reservation value in each hotel
function sumPrices() {
  let prices = [];
  listHotels.hotels.forEach((hotel) => {
    let price = 0;
    date.value.forEach((item) => {
      let day = item.getDay();
      if (day === 6 || day === 0) {
        if (customerType.value === true) {
          price = price + hotel.prices.reward_customer.weekend_price;
        } else {
          price = price + hotel.prices.regular_customer.weekend_price;
        }
      } else {
        if (customerType.value === true) {
          price = price + hotel.prices.reward_customer.weekday_price;
        } else {
          price = price + hotel.prices.regular_customer.weekday_price;
        }
      }
    });
    console.log("Precio " + hotel.name, price);
    prices.push({ hotel: hotel.code, price: price });
  });
  findLowest(prices);
}

//Find cheapest hotel
function findLowest(prices) {
  var lowest = Number.POSITIVE_INFINITY;
  var tmp;
  for (var i = prices.length - 1; i >= 0; i--) {
    tmp = prices[i].price;
    if (tmp < lowest) lowest = tmp;
  }

  var duplicated = prices.filter((i) => i.price === lowest);

  if (duplicated.length > 1) {
    findHighestRating(duplicated);
  } else {
    var hotel = listHotels.hotels.find((h) => h.code === duplicated[0].hotel);
    lowestHotel = hotel;
    showHotel.value = true;
  }
}

//Find the highest rating hotel
function findHighestRating(duplicated) {
  let hotels = [];
  duplicated.forEach((item) => {
    var hotel = listHotels.hotels.find((h) => h.code === item.hotel);
    hotels.push(hotel);
  });

  var highest = Number.NEGATIVE_INFINITY;
  var tmp;
  var hotel = {};
  for (var i = hotels.length - 1; i >= 0; i--) {
    tmp = hotels[i].rating;
    if (tmp > highest) {
      highest = tmp;
      hotel = hotels[i];
    }
  }

  lowestHotel = hotel;
  showHotel.value = true;
}
</script>

<template>
  <div class="block-title">
    <img class="icon" src="./assets/hotel-icon.png" alt="Hotel reservations" />
    <h2 class="title">Welcome to our room reservation services</h2>
  </div>
  <!-- Input values -->
  <div class="form-date">
    <div class="block-date">
      <p class="bold">Please select a date</p>
      <VueDatePicker
        class="date-picker"
        :class="{ 'date-error': errorDate }"
        v-model="date"
        multi-dates
        no-disabled-range
        :enable-time-picker="false"
        :start-date="startDate"
        placeholder="Select Date"
      ></VueDatePicker>
      <p v-if="errorDate" class="error">Select a date is required</p>
    </div>
    <div class="block-customer">
      <p class="bold">Are you a Reward Customer?</p>
      <div class="d-flex customer">
        <p class="bold">NO</p>
        <label class="switch">
          <input type="checkbox" v-model="customerType" />
          <span class="slider round"></span>
        </label>
        <p class="bold">YES</p>
      </div>
    </div>
    <div class="block-button">
      <button class="button bold" @click="validar">Search hotel</button>
    </div>
  </div>
  <!-- Input values -->
  <!-- Output value -->
  <div v-if="showHotel">
    <h1 class="green">
      The cheapest option for you is the
      <span class="bold">{{ lowestHotel.name }}</span> Hotel
    </h1>
  </div>
  <!-- Output value -->
  <!-- Grid hotels -->
  <div class="container">
    <div class="row">
      <ListHotels
        v-for="hotel in listHotels.hotels"
        :key="hotel.code"
        :hotel="hotel"
        :cheapest="hotel.code === lowestHotel.code"
      />
    </div>
  </div>
  <!-- Grid hotels -->
</template>

<style scoped>
.title {
  margin: 0px 10px;
  color: #0b2739;
}
.icon {
  width: 7%;
}
.container {
  width: 100%;
  max-width: 100vw;
}
.row {
  display: flex;
  flex-wrap: wrap;
}
.form-date {
  margin: 10px 10px 0 10px;
  width: 100%;
  display: flex;
}
.block-title {
  display: flex;
  align-items: center;
}
.block-date {
  width: 40%;
  margin: 20px 20px 20px 0px;
}
.block-customer {
  width: 25%;
  margin: 20px 20px;
}
.block-button {
  align-self: center;
}
.date-picker {
  margin: 10px 0 0 0;
  /* width: 40%; */
}
.date-error {
  border: 1px solid #dc3545;
  border-radius: 4px;
}
.customer {
  align-items: center;
  margin: 10px 0;
}
.button {
  background-color: #2196f3;
  padding: 10px 15px 10px 15px;
  margin: 10px 0;
  border-radius: 15px;
  border: none;
  color: #ffffff;
  font-size: 15px;
}
.error {
  color: #dc3545 !important;
  font-size: 13px;
}
.green {
  color: #3c7521;
  text-align: center;
  margin: 0 0 20px 0;
}

@media (max-width: 480px) {
  .icon {
    width: 19% !important;
  }
  .form-date {
    display: block;
    margin: 10px 0;
  }
  .block-date {
    width: 100%;
    margin: 15px 0;
  }
  .block-customer {
    width: 100%;
    margin: 15px 0;
  }
  .block-button {
    text-align: center !important;
  }
}
</style>