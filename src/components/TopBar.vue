<template>
  <header class="bar fixed inset-0 w-full flex bg-violet-500 text-white justify-between items-center p-4 border-b shadow z-20 ">
    <div class="text-lg font-bold"><slot name="title"></slot></div>
    <slot name="actions"></slot>
  </header>
</template>

<script>
export default {
  name: "TopBar",
};
</script>
