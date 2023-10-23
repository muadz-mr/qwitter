<template>
  <q-page class="relative-position">
    <q-scroll-area class="absolute full-height full-width">
      <div class="q-py-lg q-px-md row q-col-gutter-md items-end">
        <div class="col">
          <q-input
            v-model="newQweetContent"
            placeholder="What's happening?"
            counter
            maxlength="280"
            bottom-slots
            autogrow
            dense
            :input-style="{ fontSize: '16px', lineHeight: '1.4' }"
          >
            <template v-slot:before>
              <q-avatar size="lg" class="q-mr-sm">
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

      <q-list>
        <transition-group
          appear
          enter-active-class="animated fadeInDown"
          leave-active-class="animated fadeOutLeft"
        >
          <q-item class="qweet q-py-md" v-for="qweet in qweets" :key="qweet.id">
            <q-item-section avatar top>
              <q-avatar>
                <img src="https://cdn.quasar.dev/img/avatar4.jpg" />
              </q-avatar>
            </q-item-section>

            <q-item-section>
              <q-item-label class="text-subtitle1 q-mb-xs row">
                <strong>John Doe</strong>
                <span class="q-mx-sm text-grey-7">@john_doe</span>
                <span>
                  <span class="gt-xs q-mr-xs">&bullet;</span>
                  {{ qweet.date | relativeDate }}
                </span>
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
                  @click="toggleLiked(qweet)"
                  flat
                  round
                  size="sm"
                  :color="qweet.liked ? 'pink' : 'grey'"
                  :icon="`${qweet.liked ? 'fa-solid' : 'fa-regular'} fa-heart`"
                />
                <q-btn
                  @click="deleteQweet(qweet)"
                  flat
                  round
                  size="sm"
                  color="grey"
                  icon="fa-regular fa-trash-can"
                />
              </div>
            </q-item-section>
          </q-item>
        </transition-group>
      </q-list>
    </q-scroll-area>
  </q-page>
</template>

<script>
import db from "src/boot/firebase";
import {
  collection,
  query,
  onSnapshot,
  orderBy,
  doc,
  addDoc,
  deleteDoc,
  updateDoc,
} from "firebase/firestore";
import { formatDistanceToNow } from "date-fns";

export default {
  name: "PageHome",
  data() {
    return {
      unsubscribe: null,
      newQweetContent: "",
      qweets: [],
    };
  },
  filters: {
    relativeDate(value) {
      return formatDistanceToNow(value, { addSuffix: true });
    },
  },
  methods: {
    async addNewQweet() {
      if (!this.newQweetContent) return;
      let newQweet = {
        content: this.newQweetContent,
        date: Date.now(),
        liked: false,
      };

      try {
        const docRef = await addDoc(collection(db, "qweets"), newQweet);
      } catch (error) {
        console.error("Error adding data: ", error);
      }

      this.newQweetContent = "";
    },
    async deleteQweet(qweet) {
      try {
        await deleteDoc(doc(db, "qweets", qweet.id));
      } catch (error) {
        console.error("Error removing data: ", error);
      }
    },
    async toggleLiked(qweet) {
      const qweetRef = doc(db, "qweets", qweet.id);

      try {
        await updateDoc(qweetRef, {
          liked: !qweet.liked,
        });
      } catch (error) {
        console.error('Error updating data: ", error"');
      }
    },
  },
  mounted() {
    const q = query(collection(db, "qweets"), orderBy("date"));
    const unsubscribe = onSnapshot(
      q,
      (snapshot) => {
        snapshot.docChanges().forEach((change) => {
          let qweetChange = change.doc.data();
          qweetChange["id"] = change.doc.id;

          if (change.type === "added") {
            console.log("New qweet: ", qweetChange);
            this.qweets.unshift(qweetChange);
          }
          if (change.type === "modified") {
            console.log("Modified qweet: ", qweetChange);
            const qweet = this.qweets.find(
              (qweet) => qweet.id === qweetChange.id
            );
            Object.assign(qweet, qweetChange);
          }
          if (change.type === "removed") {
            console.log("Removed qweet: ", qweetChange);
            const index = this.qweets.findIndex(
              (qweet) => qweet.id === qweetChange.id
            );
            this.qweets.splice(index, 1);
          }
        });
      },
      (error) => {
        console.error("Error when getting snapshot: ", error);
      }
    );

    this.unsubscribe = unsubscribe;
  },
  beforeDestroy() {
    this.unsubscribe();
  },
};
</script>

<style lang="scss" scoped>
.divider {
  border-top: 1px solid;
  border-bottom: 1px solid;
  border-color: $grey-4;
}

.qweet:not(:first-child) {
  border-top: 1px solid rgba($color: #000000, $alpha: 0.12);
}

.qweet-content {
  white-space: pre-line;
}
</style>
