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
          @click="addNewQweet"
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

    <q-list separator>
      <q-item
        class="q-py-md"
        v-for="(qweet, index) in qweets"
        :key="qweet.date"
      >
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
          <q-item-label class="qweet-content text-body1">{{
            qweet.content
          }}</q-item-label>
          <div class="row justify-between q-mt-sm">
            <q-btn
              flat
              round
              size="sm"
              color="grey"
              icon="fa-regular fa-comment"
            />
            <q-btn
              flat
              round
              size="sm"
              color="grey"
              icon="fa-solid fa-retweet"
            />
            <q-btn
              flat
              round
              size="sm"
              color="grey"
              icon="fa-regular fa-heart"
            />
            <q-btn
              @click="deleteQweet(index)"
              flat
              round
              size="sm"
              color="grey"
              icon="fa-regular fa-trash-can"
            />
          </div>
        </q-item-section>

        <q-item-section side top>{{
          qweet.date | relativeDate
        }}</q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { formatDistanceToNow } from "date-fns";

export default {
  name: "PageHome",
  data() {
    return {
      newQweetContent: "",
      qweets: [
        {
          content:
            "Lorem ipsum dolor sit amet consectetur, adipisicing elit. Ipsaprovident labore assumenda voluptatibus expedita?",
          date: 1696840019273,
        },
        {
          content:
            "Deleniti unde quos saepe voluptates totam dolore asperiores earum praesentium ipsum cum ad numquam magnam sequi cumque, ut, debitis laborum, distinctio doloribus optio molestiae quaerat tempora. Non iusto nemo natus recusandae quia necessitatibus illo voluptas consequuntur.",
          date: 1696840094859,
        },
      ],
    };
  },
  filters: {
    relativeDate(value) {
      return formatDistanceToNow(value, { addSuffix: true });
    },
  },
  methods: {
    addNewQweet() {
      if (!this.newQweetContent) return;
      let newQweet = {
        content: this.newQweetContent,
        date: Date.now(),
      };
      this.qweets.unshift(newQweet);
      this.newQweetContent = "";
    },
    deleteQweet(key) {
      this.qweets.splice(key, 1);
    },
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
