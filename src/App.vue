<script setup lang="ts">
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import { reactive, provide, inject } from "vue";
import type User from "./types/User";
import type Entry from "./types/Entry";
import { userInjectionKey } from "./injectionKeys";

let entries: Entry[] = reactive([]);

const localEntries: string | null = localStorage.getItem("entries") 

if (typeof localEntries === 'string') {
    entries = reactive(JSON.parse(localEntries).map((x: Entry) => {
      return {
        id: x.id,
        body: x.body,
        emoji: x.emoji,
        createdAt: new Date(x.createdAt),
        userId: x.userId
      }

    }))
}


const user: User = reactive({
  id: 1,
  username: "pocho",
  settings: [],
});
provide(userInjectionKey, user);

const handleCreateEntry = (entry: Entry) => {
  entries.unshift(entry);
  localStorage.setItem("entries", JSON.stringify(entries));
};

const handleRemoveEntry = (entry: Entry) => {
  entries.splice(entries.findIndex(_entry => _entry.id == entry.id), 1)
  localStorage.setItem("entries", JSON.stringify(entries));
}

</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry" />
    <ul>
      <li v-for="entry in entries" :key="entry.id">
        <EntryCard @@delete="handleRemoveEntry" :entry="entry" />
      </li>
    </ul>
  </main>
</template>
