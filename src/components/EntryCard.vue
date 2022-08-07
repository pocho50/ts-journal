<script lang="ts" setup>
import DateDisplay from "./DateDisplay.vue";
import UseEmojis from "@/composables/UseEmojis";
import type Entry from "@/types/Entry";
import { userInjectionKey } from "@/injectionKeys";
import { inject } from "vue";
import DeletedIcon from "@/assets/icons/delete.svg";
import ViewText from "./ViewText.vue";

const user = inject(userInjectionKey);
const { findEmoji } = UseEmojis();
defineProps<{
  entry: Entry;
}>();

// events
const emit = defineEmits<{
  (e: "@delete", entry: Entry): void;
}>();

const handleDelete = (entry: Entry) => {
  emit("@delete", entry);
};
</script>
<template>
  <div class="entry-card">
    <div class="entry-card-body">
      <component width="75" :is="findEmoji(entry.emoji)"></component>
      <ViewText :msg="entry.body" :short-text-max="200"></ViewText>
    </div>
    <div class="entry-footer">
      <DateDisplay :date="entry.createdAt" class="mr-2" />
      |
      <span class="ml-2">{{ user?.username || "anonymous" }}</span>
      <DeletedIcon class="cursor-pointer" @click="handleDelete(entry)"></DeletedIcon>
    </div>
  </div>
</template>
