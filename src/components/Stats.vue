<script setup lang="ts">
import { compareAsc, differenceInDays, differenceInWeeks } from "date-fns";

const START_DATE = new Date("2023-07-31T00:00:00");
const END_DATE = new Date("2023-11-20T00:00:00");
const MIDTERMS_DATE = new Date("2023-09-19T00:00:00");
const FINALS_DATE = new Date("2023-11-14T00:00:00");

const now = new Date();
const isMidterm = compareAsc(now, MIDTERMS_DATE) !== -1;

const stats = [
  {
    name: "Semana",
    value: `${differenceInWeeks(now, START_DATE, { roundingMethod: "ceil" })}`,
  },
  {
    name: `${isMidterm ? "Finales" : "Parciales"} en`,
    value: `${differenceInDays(isMidterm ? FINALS_DATE : MIDTERMS_DATE, now)}`,
    unit: "días",
  },
  {
    name: "Fin de ciclo",
    value: `${differenceInDays(END_DATE, now)}`,
    unit: "días",
  },
  {
    name: "Progreso",
    value: `${(
      (differenceInDays(now, START_DATE) * 100) /
      differenceInDays(END_DATE, START_DATE)
    ).toFixed(2)}`,
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
