<template>
  <q-page>
    <div class="q-pa-md">
      <q-list bordered separator>
        <q-item v-for="entry in entries" :key="entry.id">
          <q-item-section
            class="text-weight-bold"
            :class="useAmountColorClass(entry.amount)"
          >
            {{ entry.name }}
          </q-item-section>

          <q-item-section
            class="text-weight-bold"
            :class="useAmountColorClass(entry.amount)"
            side
          >
            {{ useCurrencify(entry.amount) }}
          </q-item-section>
        </q-item>
      </q-list>
    </div>

    <q-footer class="transparent">
      <div class="row q-px-md q-py-sm shadow-up-3">
        <div class="col text-grey-7 text-h6">Balance:</div>
        <div class="col text-grey-7 text-h6 text-right">+ $3,999.00</div>
      </div>
      <div class="row q-pa-sm q-col-gutter-sm bg-primary">
        <div class="col">
          <q-input
            v-model="text"
            placeholder="Name"
            bg-color="white"
            outlined
            dense
          />
        </div>
        <div class="col">
          <q-input
            input-class="text-right"
            placeholder="Amount"
            bg-color="white"
            type="number"
            step="0.01"
            outlined
            dense
          />
        </div>
        <div class="col-auto">
          <q-btn round color="primary" icon="add" />
        </div>
      </div>
    </q-footer>
  </q-page>
</template>

<script setup>
/*
   imports
  */

import { ref } from "vue";
import { useCurrencify } from "src/use/useCurrencity";
import { useAmountColorClass } from "src/use/useAmountColorClass";

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
