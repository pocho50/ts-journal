<script lang="ts" setup>
import { ref, computed } from 'vue';

const props = defineProps<{
  msg: string,
  shortTextMax: number,
}>();

const expand = ref(false);

const viewText = computed(() => {
  const lengthMsg = props.msg.length;
  if (lengthMsg < props.shortTextMax || expand.value) return props.msg;
  return props.msg.slice(0, props.shortTextMax) + '...';
});
</script>

<template>
  <div class="entry-text">
    {{ viewText }}
    <a
      v-if="msg.length > shortTextMax"
      @click.prevent="expand = !expand"
      class="text-teal-700"
      href=""
      >{{ expand ? 'view less' : 'view more' }}</a
    >
  </div>
</template>