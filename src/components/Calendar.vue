<script setup lang="ts">
defineProps<{ end: Date }>();
import { addDays, compareDesc, formatISO, getMonth, isToday } from "date-fns";

type month = {
  number: number;
  start: string;
  end: string;
};

const months: Record<string, any> = [
  { number: 8, start: "2023-07-30T00:00:00", end: "2023-09-09T00:00:00" },
  { number: 9, start: "2023-08-27T00:00:00", end: "2023-10-07T00:00:00" },
  { number: 10, start: "2023-10-01T00:00:00", end: "2023-11-11T00:00:00" },
  { number: 11, start: "2023-10-29T00:00:00", end: "2023-12-09T00:00:00" },
].map((month) => createMonth(month));

function createMonth(params: month) {
  let currentDate = new Date(params.start);
  const days = [];
  const monthsLookup: Record<number, string> = {
    8: "Agosto",
    9: "Setiembre",
    10: "Octubre",
    11: "Noviembre",
  };

  if (!monthsLookup[params.number]) return {};

  while (compareDesc(currentDate, new Date(params.end)) !== -1) {
    const additionalParams: Record<string, any> = {};

    if (getMonth(currentDate) + 1 === params.number) {
      additionalParams.isCurrentMonth = true;
      if (isToday(currentDate)) additionalParams.isToday = true;
    }

    days.push({
      date: formatISO(currentDate, { representation: "date" }),
      ...additionalParams,
    });
    currentDate = addDays(currentDate, 1);
  }

  return {
    name: monthsLookup[params.number],
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
                  ? compareDesc(new Date(day.date), end) !== -1
                    ? 'bg-blue-50 text-gray-900'
                    : 'bg-white text-gray-900'
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
                  day.isToday && 'bg-red-700 font-semibold text-white',
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
