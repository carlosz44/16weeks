<script setup>
import { DateTime } from "luxon";

const months = [
  { number: 8, start: "2023-07-30", end: "2023-09-09" },
  { number: 9, start: "2023-08-27", end: "2023-10-07" },
  { number: 10, start: "2023-10-01", end: "2023-11-11" },
  { number: 11, start: "2023-10-29", end: "2023-12-09" },
].map((month) => createMonth(month));

function createMonth(monthParams) {
  let currentDate = DateTime.fromISO(monthParams.start);
  const days = [];
  const monthsLookup = {
    8: "Agosto",
    9: "Setiembre",
    10: "Octubre",
    11: "Noviembre",
  };
  const now = DateTime.now();

  if (!monthsLookup[monthParams.number]) return {};

  while (currentDate <= DateTime.fromISO(monthParams.end)) {
    const params = {};

    if (parseInt(currentDate.toFormat("L")) === parseInt(monthParams.number)) {
      params.isCurrentMonth = true;

      if (now.hasSame(currentDate, "day") && now.hasSame(currentDate, "year"))
        params.isToday = true;
    }
    days.push({ date: currentDate.toISODate(), ...params });
    currentDate = currentDate.plus({ days: 1 });
  }

  return {
    name: monthsLookup[monthParams.number],
    days,
  };
}
</script>

<template>
  <div class="bg-zinc-100 rounded-lg">
    <div class="mx-auto">
      <div class="relative grid grid-cols-1 gap-x-14 md:grid-cols-2">
        <section
          v-for="month in months"
          :key="month.name"
          :class="['text-center px-4 py-6 sm:px-6 lg:px-8']"
        >
          <h2 class="text-base font-semibold text-gray-900">
            {{ month.name }}
          </h2>
          <div class="mt-6 grid grid-cols-7 text-xs leading-6 text-gray-500">
            <div>D</div>
            <div>L</div>
            <div>M</div>
            <div>M</div>
            <div>J</div>
            <div>V</div>
            <div>S</div>
          </div>
          <div
            class="isolate mt-2 grid grid-cols-7 gap-px rounded-lg bg-gray-200 text-sm shadow ring-1 ring-gray-200"
          >
            <button
              v-for="(day, dayIdx) in month.days"
              :key="day.date"
              type="button"
              :class="[
                day.isCurrentMonth
                  ? 'bg-white text-gray-900'
                  : 'bg-gray-50 text-gray-400',
                dayIdx === 0 && 'rounded-tl-lg',
                dayIdx === 6 && 'rounded-tr-lg',
                dayIdx === month.days.length - 7 && 'rounded-bl-lg',
                dayIdx === month.days.length - 1 && 'rounded-br-lg',
                'relative py-1.5 hover:bg-gray-100 focus:z-10',
              ]"
            >
              <time
                :datetime="day.date"
                :class="[
                  day.isToday && 'bg-indigo-600 font-semibold text-white',
                  'mx-auto flex h-7 w-7 items-center justify-center rounded-full',
                ]"
                >{{ day.date.split("-").pop().replace(/^0/, "") }}</time
              >
            </button>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>
