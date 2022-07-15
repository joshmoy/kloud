<script setup>
import Header from "./components/Header.vue";
import Table from "./components/Table.vue";
</script>

<template>
  <div class="h-screen bg-[#F5F5F5] px-[5.2rem] pt-[7.1rem] pb-[17.2rem] max-w-[144rem]">
    <Header />
    <Table />
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
