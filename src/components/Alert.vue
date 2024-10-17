<script setup>
import { computed } from "vue";

const props = defineProps({
  alert: {
    type: Object,
    required: true,
  },
});

const isError = computed(() => props.alert.type === "error");
</script>

<template>
  <div
    :class="[isError ? 'bg-red-500' : 'bg-green-500']"
    class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
    role="alert"
  >
    <span class="block sm:inline">{{ alert.message }}</span>
  </div>
</template>
