<script setup lang="ts">
import { useRouter } from "vue-router";
import TopBar from "@/components/TopBar.vue";
import type { PostDocument, UserData, UserDocument } from "@/types/global";
import { getAuth, signOut } from "firebase/auth";
import { errorStore } from "@/utils/errorStore";
import {
  getCurrentUser,
  getUserDocument,
  getUserDocumentFromUsername,
  userStore,
} from "@/utils/userStore";
import { getPosts } from "@/utils/feedUtils";
import { ref } from "vue";
import PostComponent from "@/components/PostItem.vue";

const router = useRouter();

const props = defineProps({
  username: String,
});

function handleLogout() {
  const auth = getAuth();
  signOut(auth)
    .then(() => {
      router.push("/login");
    })
    .catch((error) => (errorStore.value = error));
}

const userDocument: UserDocument = await getUserDocumentFromUsername(
  props.username!
);
const userData = Object.values(userDocument)[0] as UserData;

const posts = ref<Array<PostDocument>>([]);
posts.value = await getPosts(Object.keys(userDocument)[0]);
</script>

<template>
  <div>
    <TopBar>
      <template #title>Profile</template>
    </TopBar>
    <div class="flex p-4 border-b mt-14">
      <img
        class="w-24 h-24"
        src="@/assets/user.png"
      />
      <div class="ml-4">
        <div class="text-2xl">{{ userData.name }}</div>
        <div class="mt-1 text-gray-500">@{{ userData.username }}</div>
      </div>
    </div>
    <div>
      <h1 class="p-4 ">Posts</h1>
      <post-component v-for="(post, i) in posts" :key="i" :post="post" />
    </div>
  </div>
</template>
