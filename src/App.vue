<template>
  <div class="container">
    <form @submit.prevent="createUser" class="card">
      <h2>Работа с базой данных</h2>

      <div class="form-control">
        <label for=""></label>
        <input type="text" id="userName" v-model.trim="userName" />
      </div>

      <button class="btn primary" :disabled="userName.length === 0">
        Создать пользователя
      </button>
    </form>

    <appUser-list-vue :users="users" @userRemove="deleteUser" />
  </div>
</template>

<script>
import axios from "axios";
import AppUserListVue from "./components/AppUserList.vue";

export default {
  data() {
    return {
      userName: "",
      users: [],
    };
  },
  mounted() {
    this.getUserList();
  },
  methods: {
    async createUser() {
      const response = await fetch(
        "https://userlistvue-default-rtdb.firebaseio.com/people.json",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            userName: this.userName,
          }),
        }
      );

      const firebaseData = await response.json();

      console.log(firebaseData);
      this.users.push({
        id: firebaseData,
        userName: this.userName,
      });
      this.userName = "";
    },

    async getUserList() {
      const { data } = await axios.get(
        "https://userlistvue-default-rtdb.firebaseio.com/people.json"
      );

      if (!data) {
        return;
      }

      let userList = Object.keys(data).map((key) => {
        return {
          id: key,
          ...data[key],
        };
      });

      this.users = userList;
    },

    async deleteUser(id) {
      console.log(id);
      await axios.delete(
        `https://userlistvue-default-rtdb.firebaseio.com/people/${id}.json`
      );
      this.users = this.users.filter((user) => user.id !== id);
    },
  },
  components: { AppUserListVue },
};
</script>
