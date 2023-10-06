<template>
  <q-layout view="lHr lpR fFf">
    <q-header bordered class="bg-white text-black">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="left = !left" />

        <q-toolbar-title class="text-weight-bold"> Qwitter </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="left" side="left" :width="283" bordered>
      <q-icon
        name="fa-solid fa-dove"
        size="lg"
        color="primary"
        class="q-pa-md"
      />

      <q-list>
        <q-item to="/" clickable v-ripple>
          <q-item-section avatar>
            <q-icon name="home" size="md" />
          </q-item-section>

          <q-item-section class="text-h6">Home</q-item-section>
        </q-item>

        <q-item to="/about" clickable v-ripple>
          <q-item-section avatar>
            <q-icon name="help" size="md" />
          </q-item-section>

          <q-item-section class="text-h6">About</q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-drawer show-if-above v-model="right" side="right" bordered>
      <!-- drawer content -->
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  data() {
    return {
      left: false,
      right: false,
    };
  },
};
</script>
