<template>
  <div class="h-screen w-full bg-table fixed top-0 left-0 flex items-center justify-center">
    <div class="w-[54.4rem] bg-white pt-[2.2rem] pr-[2.8rem] h-[28.8rem]">
      <div class="flex items-center justify-end">
        <button v-on:click="close"><CloseIcon /></button>
      </div>
      <div class="mt-[4.9rem] flex justify-center">
        <div>
          <p class="text-center text-[#061A48] text-[2rem] font-semibold">
            Are you sure you want to <br />
            Disable User
          </p>
          <div class="mt-[3.4rem] flex items-center">
            <button
              v-on:click="deleteUser"
              class="w-[12.8rem] h-[4rem] bg-[#3399FF] border border-solid border-[#3399FF] rounded-[0.4rem] text-white text-[1.6rem] mr-[2.8rem]"
            >
              Yes
            </button>
            <button v-on:click="close" class="bg-transparent text-table text-[1.6rem]">
              No, Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CloseIcon from "./icons/CloseIcon.vue";

export default {
  name: "deleteModal",
  components: {
    CloseIcon,
  },

  methods: {
    close() {
      this.$emit("toggleModalOff");
    },

    deleteUser() {
      this.$emit("deleteUser");
    },
  },
};
</script>
