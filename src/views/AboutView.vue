<script setup lang="ts">
import { useRouter } from "vue-router";

const router = useRouter();
</script>

<template>
  <div class="p-4 mx-auto flex flex-col items-center pt-12">
    <h1>About</h1>
    <div class="mt-3 text-lg">About this application</div>

    <div class="my-6 space-y-6 leading-7">
      <p>
        This application serves to fulfill the Final Project Requirement of the
        course CS 409 - Mobile Computing.
      </p>
      <div>
        <p>This application is developed by the following people:</p>
        <ul class="list-disc mt-2 ml-6 space-y-1">
          <li
            v-for="name in [
              'DAO, Lorenz',
              'DELA PEÑA, Angela',
              'LIWANAG, Louis Gabriel',
              'RODRIGO, Baron',
              'TAN, Zael',
            ]"
            :key="name"
          >
            {{ name }}
          </li>
        </ul>
      </div>
    </div>

    <button class="btn-secondary">
      <router-link to="/login">Go back</router-link>
    </button>
  </div>
</template>
