<template>
  <div class="container">
    <div class="inputs-container">
      <div class="title-container">
        <p class="subheader">Bill</p>
        <p class="error-text" min="0" v-if="bill < 1">Can't be zero</p>
      </div>
      <input
        type="number"
        v-model="bill"
        class="input"
        :class="{ error: bill < 1 }"
      />
      <p class="subheader">Select Tip %</p>
      <div class="tip-option-container">
        <div
          v-for="item in tipOptions"
          @click="(active = item.id), (relativeTip = item.tip)"
          class="tip-option"
          :class="{ active: active === item.id }"
          :key="item.id"
        >
          <p>{{ item.label }}</p>
        </div>
        <input
          class="input custom-tip"
          placeholder="Custom"
          v-model="customTip"
        />
      </div>
      <div class="title-container">
        <p class="subheader">Number of people</p>
        <p class="error-text" v-if="numberOfPeople < 1">Can't be zero</p>
      </div>
      <input
        type="number"
        v-model="numberOfPeople"
        min="0"
        class="input"
        :class="{ error: numberOfPeople < 1 }"
      />
    </div>
    <div class="outputs-container">
      <div v-for="(label, index) in ['Tip amount', 'Total']" class="output" :key="index">
        <div class="output-title">
          <p class="output-title-text">{{ label }}</p>
          <p class="output-title-subtext">/ person</p>
        </div>
        <p class="output-value">${{ getTipGetTotal(label) }}</p>
      </div>
      <button class="reset-button" @click="reset()">reset</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const bill = ref(0);
const active = ref(0);
const relativeTip = ref(0.05);
const numberOfPeople = ref(1);
const customTip = ref(0);
const tipOptions = ref([
  {
    label: "5%",
    id: 0,
    tip: 0.05,
  },
  {
    label: "10%",
    id: 1,
    tip: 0.1,
  },
  {
    label: "15%",
    id: 2,
    tip: 0.15,
  },
  {
    label: "25%",
    id: 3,
    tip: 0.25,
  },
  {
    label: "50%",
    id: 4,
    tip: 0.5,
  },
]);

function reset() {
  bill.value = 0;
  active.value = 0;
  relativeTip.value = 0.05;
  numberOfPeople.value = 1;
}

function getTipGetTotal(label) {
  let temp;
  if (label === "Tip amount") {
    temp =
      Math.round(
        (((customTip.value / 100 || relativeTip.value) * bill.value) /
          numberOfPeople.value) *
          100
      ) / 100;
  } else {
    temp =
      Math.round(
        (((1 + (customTip.value / 100 || relativeTip.value)) * bill.value) /
          numberOfPeople.value) *
          100
      ) / 100;
  }
  return !isFinite(temp) || isNaN(temp) ? 0 : temp;
}
</script>

<style scoped>
.input {
  padding: 5px 8px;
  text-align: right;
  font-size: 17px;
  font-family: "Space Mono", monospace;
  font-weight: bold;
  color: hsl(183, 100%, 15%);
  background-color: hsl(189, 41%, 97%);
  border-width: 0px;
  box-shadow: inset 0 2px 4px 0 rgb(0 0 0 / 0.05);
  border-radius: 4px;
}
.input:focus {
  outline: auto hsl(172, 67%, 45%);
}
.custom-tip.input {
  width: 70px;
  padding: 4px 11px;
}
.container {
  display: flex;
  padding: 24px;
  gap: 20px;
  background-color: hsl(0, 0%, 100%);
  border-radius: 20px;
  font-family: "Space Mono", monospace;
  width: 650px;
}
.tip-option-container {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
.tip-option {
  width: 70px;
  padding: 4px 11px;
  border-radius: 4px;
  background-color: hsl(183, 100%, 15%);
  display: flex;
  justify-content: center;
  margin: 0px;
  font-size: 18px;
  color: white;
  cursor: pointer;
  font-weight: bold;
  border-width: 0px;
}
.output {
  display: flex;
  margin-bottom: 50px;
  justify-content: space-between;
  align-items: center;
}
.output-title {
  gap: 0px;
}
.output-title-text {
  margin: 0;
}
.outputs-container {
  background-color: hsl(183, 100%, 15%);
  padding: 30px;
  border-radius: 12px;
  margin: 0px;
  flex: 3;
}
.outputs-container,
.inputs-container {
  width: 100%;
}
.inputs-container {
  gap: px;
  display: flex;
  flex-direction: column;
  flex: 4;
  gap: 6px;
}
.reset-button {
  padding: 10px 0px;
  width: 100%;
  background-color: hsl(172, 67%, 45%);
  text-transform: uppercase;
  border-radius: 4px;
  color: hsl(183, 100%, 15%);
  font-weight: bold;
  margin-top: 50px;
}
.subheader {
  color: hsl(184, 14%, 56%);
  font-size: 13px;
  font-weight: bold;
  margin-top: 30px;
}
.output-title-text {
  color: hsl(0, 0%, 100%);
  font-size: 12px;
  font-weight: bold;
}
.output-title-subtext {
  color: hsl(184, 14%, 56%);
  margin: 0px;
  font-size: 10px;
  font-weight: bold;
}
.output-value {
  font-size: 40px;
  font-weight: bold;
  color: hsl(172, 67%, 45%);
}
.active {
  background-color: hsl(185, 41%, 84%);
  color: hsl(183, 100%, 15%);
}
.error {
  border: 2px solid red;
}
.title-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.error-text {
  color: red;
  font-size: 13px;
  font-weight: bold;
  margin-top: 30px;
}
</style>