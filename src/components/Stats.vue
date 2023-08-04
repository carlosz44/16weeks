<script setup>
import { DateTime } from "luxon";

const NUMBER_OF_WEEKS = 16;
const START_DATE = DateTime.fromISO("2023-07-31");
const END_DATE = DateTime.fromISO("2023-11-20");
const MIDTERMS_DATE = DateTime.fromISO("2023-09-19");
const FINALS_DATE = DateTime.fromISO("2023-11-14");

const now = DateTime.now();
const isMidterm = now <= MIDTERMS_DATE;
const refDateTerm = isMidterm ? MIDTERMS_DATE : FINALS_DATE;

const currentWeek = Math.ceil(now.diff(START_DATE, "weeks").as("weeks"));
const stats = [
  { name: "Semana", value: `${currentWeek}` },
  {
    name: `${isMidterm ? "Parciales" : "Finales"} en`,
    value: `${Math.round(refDateTerm.diff(now, "days").as("days"))}`,
    unit: "días",
  },
  {
    name: "Fin de ciclo",
    value: `${Math.round(END_DATE.diff(now, "days").as("days"))}`,
    unit: "días",
  },
  {
    name: "Progreso",
    value: `${((currentWeek * 100) / NUMBER_OF_WEEKS).toFixed(2)}`,
    unit: "%",
  },
];
</script>

<template>
  <div class="bg-zinc-950 rounded-lg">
    <div class="mx-auto">
      <div
        class="grid grid-cols-1 gap-px divide-x divide-white/5 sm:grid-cols-2 lg:grid-cols-4"
      >
        <div
          v-for="stat in stats"
          :key="stat.name"
          class="px-4 py-6 sm:px-6 lg:px-8"
        >
          <p
            class="text-center sm:text-left text-sm font-medium leading-6 text-gray-400"
          >
            {{ stat.name }}
          </p>
          <p
            class="mt-2 flex justify-center sm:justify-normal items-baseline gap-x-2"
          >
            <span class="text-4xl font-semibold tracking-tight text-white">{{
              stat.value
            }}</span>
            <span v-if="stat.unit" class="text-sm text-gray-400">{{
              stat.unit
            }}</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
