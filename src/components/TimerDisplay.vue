<template>
  <p>{{ status }} - {{ time }}</p>
</template>

<script setup>
defineProps({
  status: String,
  time: String
})
</script>
