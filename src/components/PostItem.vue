<script setup lang="ts">
import { computed, defineProps } from "vue";
import { getUserDocument } from "@/utils/userStore";

import TimeAgo from "javascript-time-ago";
import { useRouter } from "vue-router";

const props = defineProps({
  post: {
    type: Object,
    required: true,
  },
});

const router = useRouter()

const [uid, data] = Object.entries(props.post)[0];
const user = await getUserDocument(data.user);
const { name, username } = Object.values(user as object)[0];
const timeAgo = computed(() => {
  return new TimeAgo("en-US").format(data.timestamp, "twitter-minute-now");
});

function goToProfile() {
  router.push("/" + username)
}

</script>

<template>
  <div class="p-4 border-b">
    <div class="flex">
      <img class="w-14 h-14" src="@/assets/user.png" />
      <div class="flex flex-col ml-3">
        <div class="flex flex-col" @click="goToProfile">
          <span class="font-medium">{{ name }}</span>
          <span class="text-gray-500">@{{ username }} · {{ timeAgo }}</span>
        </div>
        <p class="mt-3">{{ data.text }}</p>
      </div>
    </div>
  </div>
</template>
