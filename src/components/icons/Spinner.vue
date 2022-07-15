<template>
  <div class="w-[20rem] mx-[auto] flex justify-center">
    <div class="loadingspinner" />
  </div>
</template>

<style scoped>
.loadingspinner {
  pointer-events: none;
  margin-top: 7rem;
  width: 2.5em;
  height: 2.5em;
  border: 0.4em solid transparent;
  border-color: #eee;
  border-top-color: #3e67ec;
  border-radius: 50%;
  animation: loadingspin 1s linear infinite;
}

@keyframes loadingspin {
  100% {
    transform: rotate(360deg);
  }
}
</style>
