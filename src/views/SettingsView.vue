<script setup lang="ts">
import { useRouter } from "vue-router";
import TopBar from "@/components/TopBar.vue";
import type { PostDocument, UserData, UserDocument } from "@/types/global";
import { deleteUser, getAuth, signOut } from "firebase/auth";
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
import InputComponent from "@/components/InputComponent.vue";
import { doc, getFirestore, setDoc } from "firebase/firestore";

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

function handleDeleteUser() {
  if (confirm("Would you really like to delete your account?")) {
    const auth = getAuth();
    deleteUser(auth.currentUser!)
      .then(() => router.push("/"))
      .catch((error) => (errorStore.value = error));
  }
}

const userDocument = await getUserDocument(userStore.value!.uid);
const userData = Object.values(userDocument)[0] as UserData;
const name = ref<string>(userData.name);
const username = ref<string>(userData.username);

function handleChangeName() {
  const db = getFirestore();
  const docRef = doc(db, "users", userStore.value!.uid);
  setDoc(
    docRef,
    {
      name: name.value,
    },
    { merge: true }
  ).then(() => router.go(0));
}
</script>

<template>
  <div>
    <TopBar>
      <template #title>Settings</template>
    </TopBar>
    <div class="flex flex-col p-4 border-b mt-14">
      <h1>Account details</h1>
      <form @submit.prevent="handleChangeName">
        <input-component v-model="name">
          <template #label>Name</template>
        </input-component>
        <input-component disabled v-model="username">
          <template #label>Username</template>
          <template #helper>Your username can't be changed.</template>
        </input-component>
        <button type="submit">Apply changes</button>
      </form>
    </div>
    <div class="flex flex-col p-4 border-b pb-8">
      <h1 class="mb-4">Manage account</h1>
      <button class="btn-secondary" @click="handleLogout">Sign out</button>
      <p class="mt-6">Warning: this process is irreversible!</p>
      <button
        class="px-3 py-2 w-full font-medium bg-red-700 text-white rounded mt-4"
        @click="handleDeleteUser"
      >
        Delete user
      </button>
    </div>
  </div>
</template>
