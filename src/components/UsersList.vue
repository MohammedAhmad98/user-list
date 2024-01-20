<template>
  <div class="container">
    <div class="user-list__container">
      <h1>Users List</h1>
      <div class="user-list">
        <user-card v-for="user in users" :user="user" :key="user.email" />
      </div>
      <template v-if="loading">
        <span class="loader"></span>
      </template>
      <button @click="loadMore" v-if="hasMoreUsers" class="load-more-btn">Load More</button>
    </div>

  </div>
</template>


<script>
import axios from 'axios';
import UserCard from './UserCard.vue';
export default {
  name: "usersList",
  components: {
    UserCard
  },
  data() {
    return {
      users: [],
      page: 1,
      loading: false,
      hasMoreUsers: true,
      selectedUser: null,
    };
  },
  mounted() {
    this.fetchUsers();

  },
  methods: {
    async fetchUsers() {
      try {
        this.loading = true;
        const response = await axios.get(`https://randomuser.me/api/?page=${this.page}&results=5`);
        this.users = this.users.concat(response.data.results);
        this.page++;
        this.loading = false;
      } catch (error) {
        console.error('Error fetching users', error);
        this.loading = false;
      }
    },
    loadMore() {
      this.fetchUsers();
    },
  },

};
</script>

<style>
.container {
  width: 75%;
  margin: 3rem auto;
}

.user-list__container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}


.user-list {
  flex: 1;
  padding: 20px;
  width: 100%;
}

.load-more-btn {
  background: transparent;
  border: 1px solid #007BFF;
  border-radius: 8px;
  padding: 8px 16px;
  color: #007BFF;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
  width: 15rem;
  height: 2.5rem;
}

.loader {
  width: 48px;
  height: 48px;
  border: 5px solid #007BFF;
  border-bottom-color: transparent;
  border-radius: 50%;
  display: inline-block;
  box-sizing: border-box;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>
