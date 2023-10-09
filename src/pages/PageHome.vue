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
              <img src="https://cdn.quasar.dev/img/avatar4.jpg" />
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

    <q-list>
      <q-item class="q-py-md">
        <q-item-section avatar top>
          <q-avatar>
            <img src="https://cdn.quasar.dev/img/avatar4.jpg" />
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label class="text-subtitle1">
            <strong>John Doe</strong>
            <span class="q-ml-sm text-grey-7">@john_doe</span>
          </q-item-label>
          <q-item-label class="qweet-content text-body1"
            >Lorem ipsum dolor sit amet consectetur, adipisicing elit. Ipsa
            provident labore assumenda voluptatibus expedita? Deleniti unde quos
            saepe voluptates totam dolore asperiores earum praesentium ipsum cum
            ad numquam magnam sequi cumque, ut, debitis laborum, distinctio
            doloribus optio molestiae quaerat tempora. Non iusto nemo natus
            recusandae quia necessitatibus illo voluptas
            consequuntur.</q-item-label
          >
          <div class="row justify-between q-mt-sm">
            <q-btn flat round size="sm" color="grey" icon="fa-regular fa-comment" />
            <q-btn flat round size="sm" color="grey" icon="fa-solid fa-retweet" />
            <q-btn flat round size="sm" color="grey" icon="fa-regular fa-heart" />
            <q-btn flat round size="sm" color="grey" icon="fa-regular fa-trash-can" />
          </div>
        </q-item-section>

        <q-item-section side top> 1 min ago </q-item-section>
      </q-item>
    </q-list>
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
}

.qweet-content {
  white-space: pre-line;
}
</style>
