

<template>
  <div class="h-screen bg-[#F5F5F5] px-[5.2rem] pt-[7.1rem] pb-[17.2rem] max-w-[144rem]">
    <Header />
    <Table />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Table from "./components/Table.vue";

export default {
   components: {
    Header,
    Table
  },
}
</script>
