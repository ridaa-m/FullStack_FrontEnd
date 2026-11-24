<script setup>
import { ref, onMounted, computed } from "vue";

// Stores lessons added to the shopping cart

const cart = ref([]);

// stores all lessons fetched from the backend

const lessons = ref([]);

// Stores the selected option sorted

const sortBy = ref("topic");

// Controls whether the cart is visible

const showCart = ref(false);

// Shows or hides the cart

function toggleCart() {
  showCart.value = !showCart.value;
}

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
    <!-- Header -->

    <div class="header">
      <h1>After School Lessons</h1>

      <button class="cart-button" @click="toggleCart()">
        🛒 Cart ({{ cart.length }})
      </button>
    </div>

    <!-- Shopping Cart -->

    <div v-if="showCart" class="cart-panel">
      <h2>Shopping Cart</h2>

      <div v-if="cart.length === 0">
        <p>Your cart is empty.</p>
      </div>

      <div v-for="(item, index) in cart" :key="index" class="cart-item">
        <h3>{{ item.topic }}</h3>

        <p>£{{ item.price }}</p>

        <button @click="removeFromCart(index)">Remove</button>
      </div>
    </div>

    <!-- Sort Dropdown -->

    <div class="sort-section">
      <label for="sort"> Sort By: </label>

      <select id="sort" v-model="sortBy">
        <option value="topic">Subject</option>

        <option value="location">Location</option>

        <option value="price">Price</option>

        <option value="space">Spaces</option>
      </select>
    </div>

    <!-- Lesson Cards -->

    <div class="lesson-grid">
      <div
        class="lesson-card"
        v-for="lesson in sortedLessons"
        :key="lesson._id"
      >
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
    </div>
  </div>
</template>
