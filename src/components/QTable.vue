<template>
  <q-table
    class="q-mt-sm"
    row-key="name"
    :columns="columns"
    :rows="rows"
    separator="cell"
    flat
    bordered
  />
</template>
<script>
import { onMounted } from 'vue';

export default {
  props: {
    columns: {
      type: Array,
      default: () => [],
    },

    rows: {
      type: Array,
      default: () => [],
    },
  },

  setup(props, { emit }) {

    onMounted(() => {

    });

    return {

    };
  },
};
</script>
<style lang="scss" scoped>

</style>
