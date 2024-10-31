<script setup lang="ts">
import PostListComponent from "@/components/Post/PostListComponent.vue";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";

const { currentUsername, isLoggedIn } = storeToRefs(useUserStore());

// const userPosts = await PostListComponent.getPosts(currentUsername.value);
</script>

<template>
  <main>
    <h1>Profile</h1>
    <section>
      <h1 v-if="isLoggedIn">Welcome {{ currentUsername }}!</h1>
      <!-- List recommendations
        List user's posts -->
      <h1 v-else>
        <button>Add friend</button>
      </h1>
    </section>
    <!-- <userPosts /> -->
    <PostListComponent :own="true" :userId="$route.params.userId" />
  </main>
</template>

<style scoped>
h1 {
  text-align: center;
}
</style>
