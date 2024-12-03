<template>
  <q-page>
    <div class="q-pa-md">
      <q-list bordered separator>
        <q-item v-for="entry in entries" :key="entry.id">
          <q-item-section
            :class="[
              { 'text-positive': entry.amount > 0 },
              { 'text-negative': entry.amount < 0 },
            ]"
          >
            {{ entry.name }}
          </q-item-section>

          <q-item-section
            :class="[
              { 'text-positive': entry.amount > 0 },
              { 'text-negative': entry.amount < 0 },
            ]"
            side
          >
            {{ useCurrencify(entry.amount) }}
          </q-item-section>
        </q-item>
      </q-list>
    </div>
  </q-page>
</template>

<script setup>
/*
   imports
  */

import { ref } from "vue";
import { useCurrencify } from "src/use/useCurrencity";

/*
   entries
  */

const entries = ref([
  {
    id: "i1d",
    name: "Salary",
    amount: 4999.99,
  },
  {
    id: "i2d",
    name: "Rent",
    amount: -999,
  },
  {
    id: "i3d",
    name: "Phone",
    amount: -14.99,
  },
  {
    id: "i4d",
    name: "Unknow",
    amount: 0,
  },
]);
</script>
