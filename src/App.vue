<script lang="ts" setup>
import { ref, computed } from 'vue';
import Button from 'primevue/button';
import InputNumber from 'primevue/inputnumber';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

const history = ref([])

const miles = ref()
const mpg = ref()
const price = ref()

const gallons = computed(() => miles.value / mpg.value)
const litres = computed(() => gallons.value * 4.54609)
const cost = computed(() => litres.value * price.value / 100)
const rate = computed(() => cost.value / miles.value)

const obj = computed(() => {
  return [{
    miles: miles.value,
    mpg: mpg.value,
    price: price.value,
    litres: litres.value,
    cost: cost.value,
    rate: rate.value
  }]
})

function submit() {
  history.value = obj.value.concat(history.value)
}
</script>

<template>
  <div id="app" class="flex flex-col gap-4 m-5">
    <h1 class=" text-5xl">Mileage Calculator</h1>
    <div class="flex flex-wrap gap-2">
      <div class="flex flex-col">
        <label for="miles">Miles</label>
        <InputNumber id="miles" placeholder="50" v-model="miles" :maxFractionDigits="2" />
      </div>
      <div class="flex flex-col">
        <label for="mpg">Miles per Gallon</label>
        <InputNumber id="mpg" placeholder="33.3" v-model="mpg" :maxFractionDigits="2" />
      </div>
      <div class="flex flex-col">
        <label for="price">Price per Litre</label>
        <InputNumber id="price" placeholder="134.9" v-model="price" :maxFractionDigits="2" />
      </div>
    </div>
    <Button label="Submit" @click="submit" class="w-32" />
    <h1 v-if="history.length > 0" class="text-3xl">History</h1>
    <DataTable v-if="history.length > 0" :value="history" scrollable scrollHeight="400px"
      showGridlines stripedRows>
      <Column field="miles" header="Miles"></Column>
      <Column field="mpg" header="Miles per Gallon"></Column>
      <Column header="Price (p/litre)">
        <template #body="slotProps">
          {{ slotProps.data.price.toFixed(2) }}p
        </template>
      </Column>
      <Column header="Litres">
        <template #body="slotProps">
          {{ slotProps.data.litres.toFixed(2) }}L
        </template>
      </Column>
      <Column header="Cost">
        <template #body="slotProps">
          £{{ slotProps.data.cost.toFixed(2) }}
        </template>
      </Column>
      <Column header="Rate (£/mile)">
        <template #body="slotProps">
          £{{ slotProps.data.rate.toFixed(2) }}
        </template>
      </Column>
    </DataTable>
  </div>
</template>