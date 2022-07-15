<template>
  <div class="h-screen w-full bg-table fixed top-0 left-0 flex items-center justify-center">
    <div class="w-[54.4rem] bg-white pt-[2.2rem] h-[45.6rem]">
      <div class="flex items-center justify-end pr-[2.8rem]">
        <button v-on:click="closeAll"><CloseIcon /></button>
      </div>
      <div class="mt-[7.1rem] flex justify-center">
        <div class="w-[34rem]">
          <div class="w-[5.85rem] mx-auto"><SuccessIcon /></div>
          <p class="text-center text-[#061A48] text-[2rem] font-semibold mt-[3rem] mb-[1rem]">
            Successful
          </p>
          <p class="text-center text-[1.6rem] text-table">{{ modalMessage }}</p>
          <div class="mt-[3rem] flex items-center" v-if="!isDelete">
            <button
              v-on:click="closeAll"
              class="w-full h-[4rem] bg-[#F5FAFF] text-[#3399FF] rounded-[0.4rem] text-[1.4rem]"
            >
              View Details
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CloseIcon from "./icons/CloseIcon.vue";
import SuccessIcon from "./icons/SuccessIcon.vue";

export default {
  name: "successModal",
  props: ["modalMessage", "isDelete"],
  components: {
    CloseIcon,
    SuccessIcon,
  },

  methods: {
    closeAll() {
      this.$emit("closeAll");
    },
  },
};
</script>
