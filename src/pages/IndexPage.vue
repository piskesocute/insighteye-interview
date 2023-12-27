<script setup>
import { ref, reactive, onMounted, getCurrentInstance, toRaw } from 'vue';
import dayjs from 'dayjs';
import customParseFormat from 'dayjs/plugin/customParseFormat';
import { useQuasar } from 'quasar';
import AddDialog from '../components/AddDialog.vue';
import QTable from '../components/QTable.vue';
import DeleteDialog from '../components/DeleteDialog.vue';

dayjs.extend(customParseFormat);

const $q = useQuasar();
const state = reactive({
  columns: [
    {
      name: 'name',
      label: '姓名',
      align: 'left',
      field: 'name',
      sortable: true
    },
    {
      name: 'cellphone',
      label: '手機',
      align: 'left',
      field: 'cellphone',
      sortable: true
    },
    {
      name: 'email',
      label: '信箱',
      align: 'left',
      field: 'email',
      sortable: true
    },
    {
      name: 'gender',
      label: '性別',
      align: 'left',
      field: 'gender',
      sortable: true
    },
    {
      name: 'birthday',
      label: '生日',
      align: 'left',
      field: 'birthday',
      sortable: true
    }
  ],
  rows: []
});

const { proxy } = getCurrentInstance();
const getData = async () => {
  // get http://35.194.177.50:7777/members
  try {
    const response = await proxy.$api.get('/members');

    const dateFormate = (date) => {
      const data = dayjs(date, 'YYYY-M-DTHH:mm:ss.SSSZ');
      return data.format('YYYY-MM-DD');
    };

    response.data.members.forEach((item) => {
      item.birthday = dateFormate(item.birthday);
    });
    localStorage.setItem('tableData', JSON.stringify(response.data.members));
    state.rows = response.data.members;
  } catch (error) {
    console.err(error);
  }
};

// 新增資料
const addDialogState = ref(false);

const isDialogOpen = () => {
  addDialogState.value = false;
};

const pushData = async (data) => {
  const localData = JSON.parse(localStorage.getItem('tableData'));
  localData.unshift(data);
  localStorage.setItem('tableData', JSON.stringify(localData));
  state.rows = localData;
  $q.notify({
    color: 'green',
    textColor: 'white',
    icon: 'check',
    message: '資料已成功新增',
    position: 'top-right',
    timeout: 3000,
    actions: [
      {
        label: '知道了',
        color: 'white'
      }
    ]
  });
  isDialogOpen();
};

// 勾選資料
const selected = ref([]);
const updateList = (data) => {
  selected.value = data.map((item) => {
    return toRaw(item);
  });
};

// 刪除資料
const deleteDialogState = ref(false);

const isDeleteDialogOpen = () => {
  deleteDialogState.value = false;
};

const deleteData = async () => {
  const localData = JSON.parse(localStorage.getItem('tableData'));

  const newData = localData.filter((item) => {
    return !selected.value.some((selectItem) => {
      return item.name === selectItem.name;
    });
  });

  localStorage.setItem('tableData', JSON.stringify(newData));
  state.rows = newData;
  selected.value = [];

  isDeleteDialogOpen();
  $q.notify({
    color: 'green',
    textColor: 'white',
    icon: 'check',
    message: '資料已成功刪除',
    position: 'top-right',
    timeout: 3000,
    actions: [
      {
        label: '知道了',
        color: 'white'
      }
    ]
  });
};

// 重置資料
const resetData = () => {
  getData();
  $q.notify({
    color: 'green',
    textColor: 'white',
    icon: 'check',
    message: '資料已成功重置',
    position: 'top-right',
    timeout: 3000,
    actions: [
      {
        label: '知道了',
        color: 'white'
      }
    ]
  });
};

onMounted(() => {
  if (!localStorage.getItem('tableData')) {
    getData();
  } else {
    state.rows = JSON.parse(localStorage.getItem('tableData'));
  }
});
</script>
<template>
  <q-page class="row justify-left q-pa-lg">
    <div class="block-item">
      <div class="row justify-between items-center">
        <div class="text-h6">員工基本資訊</div>
        <div>
          <q-btn
            class="q-mr-sm"
            color="primary"
            label="新增"
            @click="addDialogState = true"
          />
          <q-btn
            :disabled="selected.length === 0"
            class="q-mr-sm"
            color="negative"
            text-color="white"
            label="刪除"
            @click="deleteDialogState = true"
          />
          <q-btn outline color="positive" label="重置資料" @click="resetData" />
        </div>
      </div>
      <AddDialog
        v-model="addDialogState"
        @cancel="isDialogOpen"
        @submit="pushData"
      />
      <DeleteDialog
        v-model="deleteDialogState"
        @cancel="isDeleteDialogOpen"
        @submit="deleteData"
        :select-data="selected"
      />
      <QTable
        :columns="state.columns"
        :rows="state.rows"
        @update:listData="updateList"
      />
    </div>
  </q-page>
</template>

<style lang="scss" scoped>
.block-item {
  min-width: 400px;
}
</style>
