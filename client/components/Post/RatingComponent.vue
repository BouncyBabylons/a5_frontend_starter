<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import { fetchy } from "../../utils/fetchy";

const rating = 0;
const maxRating = 10;
const props = defineProps(["post"]);
const emit = defineEmits(["editPost", "refreshPosts"]);
const { currentUsername } = storeToRefs(useUserStore());

const updateRating = async (starCount: number) => {
  const currentStars = document.getElementsByClassName("rating_star");
  const ratingArray = document.getElementsByClassName("rating");
  const newStars = [];
  const blankStar = document.createElement("img");
  blankStar.setAttribute("src", "@/assets/images/blank_star.svg");
  ratingArray.appendChild(blankStar);
  console.log(starCount);

  for (let i = 0; i < maxRating; i++) {
    if (i < starCount) {
      // newStars.push(<img class="rating_star" @click="updateRating(1)" src="@/assets/images/blank_star.svg" />);
    } else {
      newStars.push();
    }
  }
  try {
    await fetchy(`/api/posts/${props.post._id}`, "PATCH", {
      body: { rating: rating.value },
    });
  } catch {
    return;
  }
  emit("refreshPosts");
  emit("editPost");
};
</script>

<template>
  <svg width="300" height="200">
    <polygon points="100,10 40,198 190,78 10,78 160,198" style="fill: lime; stroke: purple; stroke-width: 5; fill-rule: evenodd" />
  </svg>
  <div class="base">
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
    </div>
  </div>
</template>

<style scoped>
p {
  margin: 0em;
}

.rating {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.rating_star {
  display: flex;
  justify-content: space-around;
  height: 20px;
  width: 20px;
  cursor: pointer;
}

menu {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  gap: 1em;
  padding: 0;
  margin: 0;
}

/* .base {
  display: flex;
  justify-content: space-between;
  align-items: center;
} */

.base article:only-child {
  margin-left: auto;
}
</style>
