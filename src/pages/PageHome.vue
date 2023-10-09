<template>
  <q-page>
    <div class="q-py-lg q-px-md row q-col-gutter-md items-end">
      <div class="col">
        <q-input
          v-model="newQweetContent"
          placeholder="What's happening?"
          counter
          maxlength="280"
          bottom-slots
          autogrow
          :input-style="{ fontSize: '19px', lineHeight: '1.4' }"
        >
          <template v-slot:before>
            <q-avatar size="xl">
              <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
            </q-avatar>
          </template>
        </q-input>
      </div>
      <div class="col col-shrink">
        <q-btn
          class="q-mb-lg"
          color="primary"
          unelevated
          rounded
          label="Qweet"
          no-caps
          :disable="!newQweetContent"
        />
      </div>
    </div>
    <q-separator class="divider" size="10px" color="grey-2"></q-separator>
  </q-page>
</template>

<script>
export default {
  name: "PageHome",
  data() {
    return {
      newQweetContent: "",
    };
  },
};
</script>

<style lang="scss" scoped>
.divider {
  border-top: 1px solid;
  border-bottom: 1px solid;
  border-color: $grey-4;
}</style>
