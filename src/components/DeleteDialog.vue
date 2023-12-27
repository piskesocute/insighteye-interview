<script setup>
import { ref, defineEmits, defineProps } from 'vue';

const props = defineProps({
  selectData: {
    type: Object,
    default: () => {}
  }
});
const emit = defineEmits(['cancel', 'submit']);

const deleteDialogState = ref(false);

const submit = () => {
  emit('submit');
  deleteDialogState.value = false;
};

const cancel = () => {
  emit('cancel');
  deleteDialogState.value = false;
};
</script>

<template>
  <q-dialog v-model="deleteDialogState">
    <q-card style="width: 500px">
      <q-card-section>
        <div class="text-weight-bold text-subtitle1 text-red">
          確定要刪除以下資料嗎？
        </div>
      </q-card-section>
      <q-card-section>
        <div class="">
          <ol class="">
            <li
              v-for="item in props.selectData"
              :key="item.name"
              class="q-mb-sm"
            >
              {{ item.name }}
            </li>
          </ol>
        </div>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn label="取消" color="primary" @click="cancel" />
        <q-btn label="確定刪除" color="red" @click="submit" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>
<style></style>
