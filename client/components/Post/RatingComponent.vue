<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import { fetchy } from "../../utils/fetchy";

const rating = 0;
const props = defineProps(["post"]);
const emit = defineEmits(["editPost", "refreshPosts"]);
const { currentUsername } = storeToRefs(useUserStore());

const updateRating = async () => {
  try {
    await fetchy("/api/posts/${props.post._id}", "PATCH", {
      body: { rating: rating.value },
    });
  } catch {
    return;
  }
  emit("refreshPosts");
};
</script>

<template>
  <div class="rating">
    <img class="rating_star" @click="updateRating(1)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(2)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(3)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(4)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(5)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(6)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(7)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(8)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(9)" src="@/assets/images/blank_star.svg" />
    <img class="rating_star" @click="updateRating(10)" src="@/assets/images/blank_star.svg" />
  </div>
</template>

<style scoped>
p {
  margin: 0em;
}

.author {
  font-weight: bold;
  font-size: 1.2em;
}

menu {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  gap: 1em;
  padding: 0;
  margin: 0;
}

.timestamp {
  display: flex;
  justify-content: flex-end;
  font-size: 0.9em;
  font-style: italic;
}

.base {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.base article:only-child {
  margin-left: auto;
}
</style>
