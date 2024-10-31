<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";
import { formatDate } from "../../utils/formatDate";

const props = defineProps(["post"]);
const title = ref(props.post.title);
const creator = ref(props.post.creator);
const rating = ref(props.post.rating);
const content = ref(props.post.content);
const emit = defineEmits(["editPost", "refreshPosts"]);

const editPost = async (title: string, creator: string, rating: number, content: string) => {
  try {
    await fetchy(`/api/posts/${props.post._id}`, "PATCH", { body: { title: title, creator: creator, rating: rating, content: content } });
  } catch (e) {
    return;
  }
  emit("editPost");
  emit("refreshPosts");
};
</script>

<template>
  <form @submit.prevent="editPost(title, creator, rating, content)">
    <p class="author">{{ props.post.author }}</p>
    <textarea id="title" v-model="title" placeholder="Title" required> </textarea>
    <textarea id="creator" v-model="creator" placeholder="Creator" required> </textarea>
    <textarea id="rating" v-model="rating" placeholder="Rating" required> </textarea>
    <textarea id="content" v-model="content" placeholder="Review" required> </textarea>
    <div class="base">
      <menu>
        <li><button class="btn-small pure-button-primary pure-button" type="submit">Save</button></li>
        <li><button class="btn-small pure-button" @click="emit('editPost')">Cancel</button></li>
      </menu>
      <p v-if="props.post.dateCreated !== props.post.dateUpdated" class="timestamp">Edited on: {{ formatDate(props.post.dateUpdated) }}</p>
      <p v-else class="timestamp">Created on: {{ formatDate(props.post.dateCreated) }}</p>
    </div>
  </form>
</template>

<style scoped>
form {
  background-color: var(--base-bg);
  display: flex;
  flex-direction: column;
  gap: 0.5em;
}

textarea {
  font-family: inherit;
  font-size: inherit;
  height: 6em;
  border-radius: 4px;
  resize: none;
}

#title {
  height: 20px;
}

#creator {
  height: 20px;
}

#content {
  background-color: pink;
}

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

.base {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.timestamp {
  display: flex;
  justify-content: flex-end;
  font-size: 0.9em;
  font-style: italic;
}
</style>
