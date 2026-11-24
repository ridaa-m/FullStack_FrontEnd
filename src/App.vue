<script setup>
import { ref, onMounted, computed } from "vue";

// Stores lessons added to the shopping cart

const cart = ref([]);

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

// Add a lesson to the cart

function addToCart(lesson) {
  if (lesson.space > 0) {
    cart.value.push(lesson);

    lesson.space--;
  }
}

// Remove a lesson from the cart

function removeFromCart(index) {
  const lesson = cart.value[index];

  lesson.space++;

  cart.value.splice(index, 1);
}
</script>

<template>
  <div class="container">
    <h1>After School Lessons</h1>

    <!-- Cart Counter -->

    <h3>Cart Items: {{ cart.length }}</h3>

    <!-- Sorting Dropdown -->

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

      <button @click="addToCart(lesson)">Add To Cart</button>
    </div>

    <!-- Shopping Cart -->

    <h2>Shopping Cart</h2>

    <div v-for="(item, index) in cart" :key="index" class="lesson-card">
      <h3>{{ item.topic }}</h3>

      <p>
        <strong>Location:</strong>

        {{ item.location }}
      </p>

      <p>
        <strong>Price:</strong>

        £{{ item.price }}
      </p>

      <button @click="removeFromCart(index)">Remove From Cart</button>
    </div>
  </div>
</template>
