<script setup>
const stars = [
  { id: "star1", value: 1 },
  { id: "star2", value: 2 },
  { id: "star3", value: 3 },
  { id: "star4", value: 4 },
  { id: "star5", value: 5 },
];
// const rate = 4;
const props = defineProps({
  rate: Number,
  code: String,
});
</script>

<template>
  <div class="rate d-flex">
    <div v-for="star in stars" :key="star.id">
      <input
        type="radio"
        :id="star.id"
        :name="star.id + props.code"
        :value="star.value"
        :checked="star.value <= props.rate"
        disabled
      />
      <label :for="star.id" title="text">{{ star.id }}</label>
    </div>
  </div>
</template>

<style scoped>
.rate {
  float: left;
  height: 46px;
  padding: 0 10px;
  margin: 0px 0 15px 0;
  pointer-events: none;
  justify-content: center;
}
.rate div {
  align-self: center;
}
.rate div:not(:checked) > input {
  position: absolute;
  top: -9999px;
}
.rate div:not(:checked) > label {
  float: right;
  width: 1em;
  overflow: hidden;
  white-space: nowrap;
  cursor: pointer;
  font-size: 30px;
  color: #ccc;
}
.rate div:not(:checked) > label:before {
  content: "★ ";
}
.rate div > input:checked ~ label {
  color: #ffc700;
}
</style>