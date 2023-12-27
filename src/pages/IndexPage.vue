<template>
  <q-page class="row justify-left q-pa-lg">
    <div class="block-item">
      <div class="row justify-between items-center">
        <div class="text-h6">員工基本資訊</div>
        <div>
          <q-btn class="q-mr-sm" color="primary" label="新增" />
          <q-btn color="white" text-color="black" label="刪除" />
        </div>
      </div>
      <QTable
        :columns="state.columns"
      />
    </div>
  </q-page>
</template>

<script>
import { reactive } from 'vue';
import QTable from 'src/components/QTable.vue';

export default {
  name: 'IndexPage',

  components: {
    QTable
  },

  setup () {
    const state = reactive({
      columns: [
        {
          name: 'name',
          label: '姓名',
          align: 'left',
          field: 'name',
        },
        {
          name: 'cellphone',
          label: '手機',
          align: 'left',
          field: 'cellphone',
        },
        {
          name: 'email',
          label: '信箱',
          align: 'left',
          field: 'email',
        },
        {
          name: 'gender',
          label: '性別',
          align: 'left',
          field: 'gender',
        },
        {
          name: 'birthday',
          label: '生日',
          align: 'left',
          field: 'birthday',
        },
      ],
      rows: [],
    });

    return {
      state,
    }
  }
};
</script>
<style lang="scss" scoped>
.block-item {
  min-width: 400px;
}
</style>
