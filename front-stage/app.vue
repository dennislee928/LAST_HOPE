<template>
  <div class="body">
    <h1>Select Time Periods</h1>
    <div class="users">
      <label>
        <input type="radio" value="Ria" v-model="user" @change="checkUser" />
        Ria
      </label>
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
    <div class="confirm">
      <button
        class="button-27"
        :disabled="!confirmEnabled"
        @click="confirmChanges"
      >
        Confirm Changes
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const user = ref("");
const confirmEnabled = ref(false);
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

const checkUser = () => {
  if (user.value === "Ria") {
    const input = window.prompt(
      "Please enter your Facebook username (Capitalization and spaces must be the same)"
    );
    if (input === "Ria Chiu") {
      confirmEnabled.value = true;
      window.alert("Success");
    } else {
      confirmEnabled.value = false;
      window.alert("Error");
    }
  } else {
    confirmEnabled.value = true;
  }
};

const confirmChanges = () => {
  // Add logic to handle confirmation of changes
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
.confirm {
  size: 25ch;
}
@keyframes shiny {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 100% 50%;
  }
}

/* CSS */
.button-27 {
  appearance: none;
  background-color: #d71ad7;
  border: 2px solid #1a1a1a;
  border-radius: 15px;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  display: inline-block;
  font-family: Roobert, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  font-size: 16px;
  font-weight: 600;
  line-height: normal;
  margin: 0;
  min-height: 60px;
  min-width: 0;
  outline: none;
  padding: 16px 24px;
  text-align: center;
  text-decoration: none;
  transition: all 300ms cubic-bezier(0.23, 1, 0.32, 1);
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  width: 100%;
  will-change: transform;
}

.button-27:disabled {
  pointer-events: none;
}

.button-27:hover {
  box-shadow: rgba(0, 0, 0, 0.25) 0 8px 15px;
  transform: translateY(-2px);
}

.button-27:active {
  box-shadow: none;
  transform: translateY(0);
}
</style>
