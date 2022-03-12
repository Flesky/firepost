<script setup lang="ts">
defineProps({
  modelValue: String,
  feedback: String,
  id: String,
  type: {
    type: String,
    default: "text",
  },
});

defineEmits(["update:modelValue"]);
</script>

<template>
  <label>
    <span><slot name="label"></slot></span>
    <input
      :id="id"
      :type="type"
      ref="input"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
    <div v-if="$slots.helper" class="helper"><slot name="helper"></slot></div>
    <div class="feedback" v-if="feedback">{{ feedback }}</div>
  </label>
</template>

<style scoped>
label {
  @apply space-y-2 mt-6;
}
input {
  @apply w-full border focus:outline-none rounded px-3 py-2 focus:ring-purple-500 focus:ring-1 focus:border-purple-500;
}
.helper {
  @apply text-gray-500;
}
.feedback {
  @apply text-red-500;
}
</style>
