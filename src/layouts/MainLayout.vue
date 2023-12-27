<script>
export default {
  name: 'MainLayout',

  setup() {
    return {};
  }
};
</script>

<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
