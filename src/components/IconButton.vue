<template>
    <button
      class="w-full h-full rounded flex items-center justify-center p-2"
      @click="navigate"
      :class="[isActive ? 'bg-violet-500 fill-white' : '' ]"
    >
     <slot> </slot></button
  >
</template>

<script>
export default {
  props: {
    isActive: Boolean,
  },
};
</script>
