<template>
  <div class="h-screen w-full bg-table fixed top-0 left-0 flex items-center justify-center">
    <div class="w-[95.6rem] bg-white">
      <div
        class="h-[6rem] flex items-center justify-between pl-[3.6rem] pr-[3.1rem] border border-solid border-[#E1E5EE]"
      >
        <p class="text-[1.6rem] text-table font-semibold">
          {{ isEdit ? `Edit User - ` + userDetails.name : "Create New User(s)" }}
        </p>
        <button v-on:click="close"><CloseIcon /></button>
      </div>
      <div class="pt-[7.8rem] pb-[5.9rem] px-[13.8rem]">
        <form @submit.prevent="saveDetails">
          <div class="mb-[1.85rem] w-full">
            <label for="name" class="text-[1.2rem] text-table mb-[0.65rem] block">NAME *</label>
            <input
              type="text"
              id="name"
              v-model="name"
              placeholder="Enter first name"
              required
              class="h-16 px-8 border border-solid border-[#C2C9D1] w-full rounded-[0.4rem]"
            />
          </div>
          <div class="flex items-center justify-between mb-[11.6rem]">
            <div class="w-[33rem]">
              <label for="email" class="text-[1.2rem] text-table mb-[0.65rem] block">EMAIL *</label>
              <input
                type="email"
                id="email"
                v-model="email"
                placeholder="Enter email"
                required
                class="h-16 px-8 border border-solid border-[#C2C9D1] w-full rounded-[0.4rem]"
              />
            </div>
            <div class="w-[33rem]">
              <label for="phone" class="text-[1.2rem] text-table mb-[0.65rem] block"
                >PHONE NUMBER *</label
              >
              <input
                type="text"
                id="phone"
                v-model="phone"
                placeholder="Enter phone number"
                required
                class="h-16 px-8 border border-solid border-[#C2C9D1] w-full rounded-[0.4rem]"
              />
            </div>
          </div>
          <div class="flex justify-end">
            <button
              v-if="isEdit"
              type="button"
              v-on:click="handleUserDelete"
              class="w-[12rem] h-[4rem] text-[#FF5F58] bg-transparent border border-solid border-[#FF5F58] rounded-[0.4rem] text-[1.6rem] mr-[1.5rem]"
            >
              Delete User
            </button>
            <button
              type="submit"
              class="w-[17.2rem] h-[4rem] bg-[#3399FF] border border-solid border-[#3399FF] rounded-[0.4rem] text-white text-[1.6rem] font-semibold"
            >
              {{ isEdit ? "Save  Changes" : "Create User" }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import CloseIcon from "./icons/CloseIcon.vue";

export default {
  name: "modal",
  props: ["isEdit", "userDetails"],
  components: {
    CloseIcon,
  },
  created() {
    if (this.isEdit) {
      this.name = this.userDetails.name;
      this.email = this.userDetails.email;
      this.phone = this.userDetails.phone;
    }
  },
  data() {
    return {
      isLoading: false,
      name: "",
      email: "",
      phone: "",
    };
  },
  methods: {
    close() {
      this.$emit("toggleModalOff");
    },

    handleUserDelete() {
      this.$emit("deleteUser", this.userDetails);
    },

    saveDetails() {
      this.isEdit
        ? this.$emit("editDetails", {
            id: this.userDetails.id,
            name: this.name,
            phone: this.phone,
            email: this.email,
          })
        : this.$emit("saveDetails", { name: this.name, phone: this.phone, email: this.email });
      this.name = "";
      this.email = "";
      this.phone = "";
    },
  },
};
</script>
