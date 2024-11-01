<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { formatDate } from "@/utils/formatDate";
import { storeToRefs } from "pinia";
import { fetchy } from "../../utils/fetchy";
// import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

const props = defineProps(["post"]);
const emit = defineEmits(["editPost", "refreshPosts", "addComment"]);
const { currentUsername, isLoggedIn } = storeToRefs(useUserStore());

const deletePost = async () => {
  try {
    await fetchy(`/api/posts/${props.post._id}`, "DELETE");
  } catch {
    return;
  }
  emit("refreshPosts");
};

const navigateToProfile = async () => {
  const user = props.post.author;
  try {
    await router.push({ path: `profile/${user}` });
  } catch {
    return;
  }
};
</script>

<template>
  <p>{{ props.post.title }} by {{ props.post.creator }}</p>
  <p>{{ props.post.rating }}/10</p>
  <p>{{ props.post.content }}</p>
  <div class="base">
    <menu v-if="props.post.author == currentUsername">
      <li><button class="btn-small pure-button" @click="emit('editPost', props.post._id)">Edit</button></li>
      <li><button class="button-error btn-small pure-button" @click="deletePost">Delete</button></li>
    </menu>
    <menu v-else-if="isLoggedIn && props.post.author !== currentUsername">
      <li><button class="btn-small pure-button" @click="emit('addComment', props.post._id)">Comment</button></li>
    </menu>
    <article class="timestamp">
      <p v-if="props.post.dateCreated !== props.post.dateUpdated">Edited on: {{ formatDate(props.post.dateUpdated) }}</p>
      <p v-else>Posted on: {{ formatDate(props.post.dateCreated) }}</p>
    </article>
    <p class="author" @click="navigateToProfile">
      <i> Posted by: &nbsp;</i> <b> {{ props.post.author }} </b>
    </p>
  </div>
</template>

<style scoped>
p {
  margin: 0em;
}

.author {
  display: flex;
  justify-content: flex-end;
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

button.comment {
  color: #455757;
  background-color: #19f0c9;
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
