<script setup lang="ts">
import { ref } from "vue";

const props = defineProps(["post"]);
const target = ref(props.post._id);
const content = ref(props.post.content);
const emit = defineEmits(["editPost", "refreshPosts", "addComment"]);

const addComment = async (target: string, content: string) => {
  try {
    await fetchy(`/api/comments/`, "POST", { body: { target: target, content: content } });
  } catch (e) {
    return;
  }
  emit("addComment");
};
</script>

<template>
  <form @submit.prevent="addComment(target, content)"></form>
  <textarea id="comment" v-model="content" placeholder="Add a comment :)" required> </textarea>
  <div class="base">
    <menu>
      <li><button class="btn-small pure-button-primary pure-button" type="submit">Send</button></li>
      <li><button class="btn-small pure-button">Cancel</button></li>
    </menu>
  </div>
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

p {
  margin: 0em;
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
</style>
