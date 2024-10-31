<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const title = ref("");
const creator = ref("");
const rating = ref(0);
const content = ref("");
const recommendation = ref("");
const emit = defineEmits(["refreshPosts"]);

const createPost = async () => {
  try {
    await fetchy("/api/posts", "POST", {
      body: { title: title.value, creator: creator.value, rating: rating.value, content: content.value },
    });
  } catch (_) {
    return;
  }
  emit("refreshPosts");
  emptyForm();
};

const emptyForm = () => {
  title.value = "";
  creator.value = "";
  rating.value = 0;
  content.value = "";
};
</script>

<template>
  <form @submit.prevent="createPost">
    <label for="title">Title</label>
    <textarea id="title" v-model="title" placeholder="Give the title of your entertainment unit!" required> </textarea>
    <label for="creator">Creator:</label>
    <textarea id="creator" v-model="creator" placeholder="Who created the entertainment unit?" required> </textarea>
    <label for="rating">Rating:</label>
    <input type="number" id="rating" v-model="rating" placeholder="Give it a rating!" required />
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
    <label for="content">Review:</label>
    <textarea id="content" v-model="content" placeholder="What was cool? What wasn't cool?" required> </textarea>
    <label for="recommendation">Recommendation:</label>
    <textarea id="recommendation" v-model="recommendation" placeholder="Who might like this?"> </textarea>
    <button type="submit" class="pure-button-primary pure-button">Create Post</button>
  </form>
</template>

<style scoped>
.rating_star {
  cursor: pointer;
  height: 25px;
  width: 25px;
}

form {
  background-color: var(--base-bg);
  border-radius: 1em;
  display: flex;
  flex-direction: column;
  gap: 0.5em;
  padding: 1em;
}

#title {
  height: 20px;
}

#creator {
  height: 20px;
}

#content {
}

textarea {
  font-family: inherit;
  font-size: inherit;
  height: 6em;
  padding: 0.5em;
  border-radius: 4px;
  resize: none;
}
</style>
