<script setup>
import { ref, reactive, defineEmits } from 'vue';
import dayjs from 'dayjs';

const emit = defineEmits(['cancel', 'submit']);

const addDialogState = ref(false);

const data = reactive({
  name: '',
  cellphone: '',
  email: '',
  gender: 'male',
  birthday: dayjs().format('YYYY-MM-DD HH:mm')
});

const sexOption = [
  { label: '男', value: 'male' },
  { label: '女', value: 'female' },
  { label: '其他', value: 'other' }
];

const reset = () => {
  data.name = '';
  data.cellphone = '';
  data.email = '';
  data.gender = '';
  data.birthday = dayjs().format('YYYY-MM-DD HH:mm');
};

const onSubmit = () => {
  emit('submit', data);
  reset();
  addDialogState.value = false;
};

const cancel = () => {
  emit('cancel');
  reset();
  addDialogState.value = false;
};
</script>

<template>
  <q-dialog v-model="addDialogState">
    <q-card style="width: 500px">
      <q-form @submit="onSubmit" class="q-gutter-md">
        <q-card-section>
          <div class="text-h6 text-weight-bold">新增資料</div>
        </q-card-section>
        <q-card-section>
          <div>
            <div class="row q-col-gutter-md">
              <div class="col-12 col-md-6">
                <q-input
                  v-model="data.name"
                  label="姓名"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || '姓名欄位為必填',
                    (val) => (val && val.length < 10) || '姓名需小於十個字元'
                  ]"
                />
              </div>
              <div class="col-12 col-md-6">
                <q-input
                  v-model="data.cellphone"
                  label="手機"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || '手機號碼欄位為必填',
                    (val) => /^09\d{8}$/.test(val) || '手機號碼須為10碼'
                  ]"
                />
              </div>
              <div class="col-12">
                <q-input
                  v-model="data.email"
                  label="電子信箱"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || '信箱欄位為必填',
                    (val) =>
                      /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(val) ||
                      '電子信箱地址格式錯誤'
                  ]"
                />
              </div>
              <div class="col-12">
                性別
                <div class="q-pa-sm">
                  <q-option-group
                    v-model="data.gender"
                    :options="sexOption"
                    color="primary"
                    inline
                  />
                </div>
              </div>
              <div class="col-12">
                <q-input
                  outlined
                  v-model="data.birthday"
                  label="生日"
                  :rules="[
                    (val) => {
                      if (!val) {
                        return '生日日期與時間為必填';
                      } else if (!/\d{4}-\d{2}-\d{2}/.test(val)) {
                        return '日期為必填';
                      } else if (!/\d{2}:\d{2}/.test(val)) {
                        return '時間為必填';
                      } else {
                        return true;
                      }
                    }
                  ]"
                >
                  <template v-slot:prepend>
                    <q-icon name="event" class="cursor-pointer">
                      <q-popup-proxy
                        cover
                        transition-show="scale"
                        transition-hide="scale"
                        ref="qDateProxy"
                      >
                        <q-date
                          v-model="data.birthday"
                          mask="YYYY-MM-DD HH:mm"
                          @update:modelValue="() => $refs.qDateProxy.hide()"
                        >
                          <div class="row items-center justify-end">
                            <q-btn
                              v-close-popup
                              label="Close"
                              color="primary"
                              flat
                            />
                          </div>
                        </q-date>
                      </q-popup-proxy>
                    </q-icon>
                  </template>

                  <template v-slot:append>
                    <q-icon name="access_time" class="cursor-pointer">
                      <q-popup-proxy
                        cover
                        transition-show="scale"
                        transition-hide="scale"
                        ref="qTimeProxy"
                      >
                        <q-time
                          v-model="data.birthday"
                          mask="YYYY-MM-DD HH:mm"
                          format24h
                        >
                          <div class="row items-center justify-end">
                            <q-btn
                              v-close-popup
                              label="Close"
                              color="primary"
                              flat
                            />
                          </div>
                        </q-time>
                      </q-popup-proxy>
                    </q-icon>
                  </template>
                </q-input>
              </div>
            </div>
          </div>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn label="取消" color="primary" @click="cancel" />
          <q-btn label="送出" color="primary" type="submit" />
        </q-card-actions>
      </q-form>
    </q-card>
  </q-dialog>
</template>
<style></style>
