<script setup>
defineProps({
  width: {
    type: String,
    default: ''
  },
  height: {
    type: String,
    default: ''
  },
  borderRadius: {
    type: String,
    default: ''
  },
  margin: {
    type: String,
    default: ''
  }
})
</script>

<template>
<div class="placeholder">

</div>
</template>

<style scoped>
@keyframes shimmer-200 {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}

.placeholder {
  width: v-bind(width);
  height: v-bind(height);
  border-radius: v-bind(borderRadius);
  margin: v-bind(margin);
  background: linear-gradient(135deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: shimmer-200 1.2s infinite;
}
</style>
