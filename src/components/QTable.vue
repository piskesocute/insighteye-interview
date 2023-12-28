<script setup>
import { ref, watch, defineEmits, defineProps } from 'vue';

const props = defineProps({
  columns: {
    type: Array,
    default: () => []
  },

  rows: {
    type: Array,
    default: () => []
  }
});

const emit = defineEmits(['update:listData']);

const filter = ref('');

const selected = ref([]);

watch(
  () => props.rows,
  (val) => {
    selected.value = [];
  }
);

watch(
  () => selected.value,
  (val) => {
    emit('update:listData', val);
  }
);
</script>

<template>
  <q-table
    selection="multiple"
    v-model:selected="selected"
    class="q-mt-sm"
    row-key="email"
    :columns="columns"
    :rows="rows"
    separator="cell"
    flat
    bordered
    :filter="filter"
    no-data-label="找不到資料，請重新整理"
    no-results-label="搜尋不到資料，請重新輸入"
  >
    <template v-slot:top-right>
      <q-input
        borderless
        dense
        debounce="300"
        v-model="filter"
        placeholder="...想找什麼?"
      >
        <template v-slot:append>
          <q-icon name="search" />
        </template>
      </q-input>
    </template>
    <template v-slot:header-selection="props">
      <q-checkbox v-model="props.selected" />
    </template>

    <template v-slot:body="props">
      <q-tr :props="props">
        <q-td>
          <q-checkbox v-model="props.selected" />
        </q-td>

        <q-td v-for="col in props.cols" :key="col.name" :props="props">
          <q-tooltip
            anchor="bottom middle"
            self="center middle"
            transition-show="scale"
            transition-hide="scale"
          >
            {{ col.value }}
          </q-tooltip>
          {{ col.value }}
        </q-td>
      </q-tr>
    </template>
  </q-table>
</template>

<style lang="scss" scoped></style>
