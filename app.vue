<template>
  <div class="body">
    <h1>Select Time Periods</h1>
    <div class="users">
      <label> <input type="radio" value="Ria" v-model="user" /> Ria </label>
      <label>
        <input type="radio" value="Dennis" v-model="user" /> Dennis
      </label>
    </div>
    <div v-for="day in days" :key="day" class="day">
      <h2>{{ day }}</h2>
      <div
        v-for="period in periods"
        :key="period"
        :class="getClass(day, period)"
        @click="toggleSelection(day, period)"
      >
        {{ period }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const user = ref("");
const selections = ref({
  Ria: {},
  Dennis: {},
});

const days = Array.from({ length: 39 }, (_, i) =>
  new Date(2024, 7, 1 + i).toLocaleDateString()
);
const periods = [
  "Morning (8:00-12:00)",
  "Afternoon (13:00-18:00)",
  "Night (19:00-24:00)",
  "Late Night (24:00-06:00)",
];

const toggleSelection = (day, period) => {
  if (!user.value) return;
  if (!selections.value[user.value][day]) {
    selections.value[user.value][day] = new Set();
  }
  if (selections.value[user.value][day].has(period)) {
    selections.value[user.value][day].delete(period);
  } else {
    selections.value[user.value][day].add(period);
  }
};

const getClass = (day, period) => {
  const riaSelected = selections.value.Ria[day]?.has(period);
  const dennisSelected = selections.value.Dennis[day]?.has(period);
  if (riaSelected && dennisSelected) return "shiny";
  if (riaSelected) return "pink";
  if (dennisSelected) return "yellow";
  return "";
};
</script>

<style>
.body {
  align-items: center;
  text-align: center;
}
.day {
  margin-bottom: 20px;
}
.day h2 {
  margin: 0;
}
.day div {
  cursor: pointer;
  padding: 10px;
  margin: 5px;
  border: 1px solid #ccc;
  display: inline-block;
  min-width: 150px;
  text-align: center;
}
.pink {
  background-color: pink;
}
.yellow {
  background-color: yellow;
}
.shiny {
  background: linear-gradient(45deg, pink, yellow);
  color: #fff;
  animation: shiny 1s infinite;
}
.users {
  border-radius: 5px;
  border-width: 4px;
  border-style: solid;
  padding: 0px;
  border-color: crimson;
}
@keyframes shiny {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 100% 50%;
  }
}
</style>
