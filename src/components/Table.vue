<template>
  <div class="bg-white">
    <div
      class="h-24 px-[2.1rem] flex items-center justify-between border border-solid border-[#E1E5EE]"
    >
      <p class="text-[1.6rem] text-table font-semibold">View All Users</p>
      <button
        v-on:click="toggleModalOn"
        class="w-[15.8rem] h-[3.6rem] bg-[#3399FF] border border-solid border-[#3399FF] rounded-[0.4rem] text-white text-[1.6rem]"
      >
        Create New User
      </button>
    </div>
    <div class="pt-[2.2rem] pr-[2.2rem] pb-[9.3rem] pl-[1.9rem]">
      <div class="h-[5rem] px-[4.3rem] flex items-center bg-[#F1F2F5] rounded-[0.4rem]">
        <div
          class="flex items-center justify-center h-[3.5rem] w-[3.5rem] bg-white rounded-[0.2rem]"
        >
          <Search />
        </div>
      </div>
      <div v-if="isLoading">
        <Spinner />
      </div>
      <table class="mt-4 w-full" v-else>
        <thead
          class="h-16 bg-[#F8F9FA] text-[1.2rem] text-table border border-solid border-[#E1E5EE]"
        >
          <tr>
            <th class="font-semibold text-left pl-[4.3rem]">S/N</th>
            <th class="font-semibold">NAME</th>
            <th class="font-semibold text-left">EMAIL</th>
            <th class="font-semibold text-left">PHONE NUMBER</th>
            <th class="font-semibold text-left">ACTIONS</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="h-24 border border-solid border-[#E1E5EE]"
            v-for="(user, index) in users"
            :key="user.id"
          >
            <td class="text-[1.4rem] text-table pl-[4.3rem]">{{ index + 1 }}</td>
            <td>
              <div class="flex items-center justify-center">
                <div
                  class="flex items-center justify-center h-[2.8rem] w-[2.8rem] rounded-full bg-[#4780F8] text-[1.2rem] text-white mr-[1.4rem]"
                >
                  <p>{{ getInitials(user.name) }}</p>
                </div>
                <p class="text-[#0D3693] text-[1.4rem]">{{ user.name }}</p>
              </div>
            </td>
            <td class="text-[1.4rem] text-table">{{ user.email }}</td>
            <td class="text-[1.4rem] text-table">{{ user.phone }}</td>
            <td>
              <div class="flex items-center">
                <button
                  v-on:click="handleEdit(user)"
                  class="h-[2.8rem] w-[2.8rem] rounded-[0.4rem] bg-[#F5FAFF] flex justify-center items-center"
                >
                  <EditIcon />
                </button>
                <button
                  v-on:click="handleDelete(user)"
                  class="h-[2.8rem] w-[2.8rem] rounded-[0.4rem] bg-[#FEF5F4] ml-[0.8rem] flex justify-center items-center"
                >
                  <DeleteIcon />
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-if="showModal">
      <Modal
        @toggleModalOff="toggleModalOff"
        @saveDetails="createUser"
        @editDetails="editUser"
        @deleteUser="handleDelete"
        :isEdit="isEdit"
        :userDetails="user"
      />
    </div>
    <div v-if="showDeleteModal">
      <DeleteModal @deleteUser="deleteUser" @toggleModalOff="toggleDeleteModalOff" />
    </div>
    <div v-if="showSuccessModal">
      <SuccessModal @closeAll="closeAll" :modalMessage="modalMessage" :isDelete="isDelete" />
    </div>
  </div>
</template>

<script>
import Search from "./icons/Search.vue";
import EditIcon from "./icons/EditIcon.vue";
import DeleteIcon from "./icons/DeleteIcon.vue";
import axios from "axios";
import Spinner from "./icons/Spinner.vue";
import Modal from "./Modal.vue";
import DeleteModal from "./DeleteModal.vue";
import SuccessModal from "./SuccessModal.vue";

export default {
  name: "table",
  components: {
    Search,
    EditIcon,
    DeleteIcon,
    Spinner,
    Modal,
    DeleteModal,
    SuccessModal,
  },
  created() {
    this.getUsers();
  },
  data() {
    return {
      isLoading: false,
      users: [],
      showModal: false,
      user: {},
      isEdit: false,
      showDeleteModal: false,
      showSuccessModal: false,
      modalMessage: "",
      isDelete: false,
    };
  },
  methods: {
    closeAll() {
      this.toggleModalOff();
      this.toggleDeleteModalOff();
      this.toggleSuccessModalOff();
    },

    toggleSuccessModalOff() {
      this.showSuccessModal = false;
      this.isDelete = false;
    },

    toggleDeleteModalOn() {
      this.showDeleteModal = true;
    },

    toggleDeleteModalOff() {
      this.showDeleteModal = false;
      this.user = {};
    },

    toggleModalOn() {
      this.showModal = true;
    },

    toggleModalOff() {
      this.showModal = false;
      this.isEdit = false;
      this.user = {};
    },

    handleEdit(user) {
      this.user = user;
      this.isEdit = true;
      this.toggleModalOn();
    },

    handleDelete(user) {
      this.user = user;
      this.toggleDeleteModalOn();
    },

    createUser(user) {
      this.users = [...this.users, { ...user, id: this.users.length + 1 }];
      this.toggleModalOff();
      this.modalMessage = "You have successfully created a new user";
      this.showSuccessModal = true;
    },

    editUser(user) {
      const newUsersArray = this.users.map((el, id) => {
        if (user.id === el.id) {
          return (this.users[id] = user);
        }
        return el;
      });
      this.users = newUsersArray;
      this.toggleModalOff();
      this.modalMessage = "You have successfully updated the user details";
      this.showSuccessModal = true;
    },

    deleteUser() {
      const newUsersArray = this.users.filter((el) => el.id !== this.user.id);
      this.users = newUsersArray;
      this.toggleDeleteModalOff();
      this.isDelete = true;
      this.modalMessage = "You have successfully deleted this user ";
      this.showSuccessModal = true;
    },

    async getUsers() {
      this.isLoading = true;

      try {
        const response = await axios.get(`https://jsonplaceholder.typicode.com/users`);
        if (response.status === 200) {
          this.users = response.data;
          this.isLoading = false;
        }
      } catch (error) {
        console.log(error);
        this.isLoading = false;
      }
    },
    getInitials(name) {
      const splitNames = name.split(" ");
      const firstName = splitNames[splitNames.length - 2].substring(0, 1);
      const lastName = splitNames[splitNames.length - 1].substring(0, 1);
      const initials = firstName + lastName;
      return initials;
    },
  },
};
</script>
