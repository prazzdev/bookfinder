<template>
  <div class="spinner" />
</template>

<style scoped>
.spinner {
  border: 8px solid rgba(0, 0, 0, 0.1); /* Background color */
  border-left-color: #3498db; /* Spinner color */
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
