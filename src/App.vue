<script setup>
import { ref, onMounted, computed } from "vue";

// stores all lessons fetched from the backend
const lessons = ref([]);

// Stores the selected option sorted
const sortBy = ref("topic");

// Sort Lessons

const sortedLessons = computed(() => {
  return [...lessons.value].sort((a, b) => {
    if (typeof a[sortBy.value] === "string") {
      return a[sortBy.value].localeCompare(b[sortBy.value]);
    }

    return a[sortBy.value] - b[sortBy.value];
  });
});

// Fetch Lessons From Backend

onMounted(async () => {
  try {
    const response = await fetch(
      "https://m00994466-fullstack-backend.onrender.com/lessons",
    );

    lessons.value = await response.json();
  } catch (error) {
    console.error(error);
  }
});
</script>

<template>
  <div class="container">
    <h1>After School Lessons</h1>

    <!-- Dropdown for sorting -->

    <label for="sort">Sort By:</label>

    <select id="sort" v-model="sortBy">
      <option value="topic">Subject</option>
      <option value="location">Location</option>
      <option value="price">Price</option>
      <option value="space">Spaces</option>
    </select>

    <!-- Lesson Cards -->

    <div class="lesson-card" v-for="lesson in sortedLessons" :key="lesson._id">
      <h2>{{ lesson.topic }}</h2>

      <p>
        <strong>Location:</strong>
        {{ lesson.location }}
      </p>

      <p>
        <strong>Price:</strong>
        £{{ lesson.price }}
      </p>

      <p>
        <strong>Spaces:</strong>
        {{ lesson.space }}
      </p>

      <button>Add to Cart</button>
    </div>
  </div>
</template>
