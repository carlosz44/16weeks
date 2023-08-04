<script setup>
import { DateTime } from "luxon";

const months = [
  createMonths(
    8,
    DateTime.fromISO("2023-07-31"),
    DateTime.fromISO("2023-09-10")
  ),
];
console.log(months);

function createMonths(monthNumber, startDate, endDate) {
  let currentDate = startDate;
  const days = [];
  const monthsLookup = {
    8: "Agosto",
    9: "Setiembre",
    10: "Octubre",
    11: "Noviembre",
  };
  const now = DateTime.now();

  if (!monthsLookup[monthNumber]) return {};

  while (currentDate <= endDate) {
    const params = {};

    if (parseInt(currentDate.toFormat("L")) === parseInt(monthNumber)) {
      params.isCurrentMonth = true;

      if (now.hasSame(currentDate, "day") && now.hasSame(currentDate, "year"))
        params.isToday = true;
    }
    days.push({ date: currentDate.toISODate(), ...params });
    currentDate = currentDate.plus({ days: 1 });
  }

  return {
    name: monthsLookup[monthNumber],
    days,
  };
}
</script>

<template>
  <div class="">CALENDAR</div>
</template>
