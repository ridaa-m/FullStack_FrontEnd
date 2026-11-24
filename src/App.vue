<script setup>

//vue imports
import { ref, onMounted } from 'vue'

//Fetches lessons
const lessons = ref([])
//Fetches lessons from the backend API when the component is mounted
onMounted(async () => {
try {
const response = await fetch(
'https://m00994466-fullstack-backend.onrender.com/lessons'
)
lessons.value = await response.json()
} catch (error) {
console.error(error)
}
})
</script>

<template>
  
<div>

<h1>After School Lessons</h1>

<div
 v-for="lesson in lessons"
:key="lesson._id"
  >
  <h2>{{ lesson.topic }}</h2>
  <p>Location: {{ lesson.location }}</p>
  <p>Price: £{{ lesson.price }}</p>
  <p>Spaces: {{ lesson.space }}</p>
</div>
</div>
</template>