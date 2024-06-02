<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      users: [],
      page: 1,
      totalPages: 2 // Total pages set to 2 as per your requirement
    }
  },
  created() {
    this.fetchUsers()
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await axios.get(`https://reqres.in/api/users?page=${this.page}`)
        this.users = response.data.data
        this.totalPages = response.data.total_pages // Update totalPages dynamically
      } catch (error) {
        console.error('Error fetching users:', error)
      }
    },
    nextPage() {
      if (this.page < this.totalPages) {
        this.page++
        this.fetchUsers()
      }
    },
    prevPage() {
      if (this.page > 1) {
        this.page--
        this.fetchUsers()
      }
    }
  }
}
</script>
<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">Users List</h1>
    <div class="overflow-x-auto">
      <table class="min-w-full bg-white border border-gray-300">
        <thead>
          <tr>
            <th class="px-4 py-2 border-b">ID</th>
            <th class="px-4 py-2 border-b">Name</th>
            <th class="px-4 py-2 border-b">Email</th>
            <th class="px-4 py-2 border-b">Avatar</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="user in users"
            :key="user.id"
            class="hover:bg-gray-100 odd:bg-gray-100 even:bg-white"
          >
            <td class="px-4 py-2 border-b">{{ user.id }}</td>
            <td class="px-4 py-2 border-b">
              {{ user.first_name }} {{ user.last_name }}
              <a :href="'mailto:' + user.email" class="text-blue-500 ml-2">(Contact)</a>
            </td>
            <td class="px-4 py-2 border-b">{{ user.email }}</td>
            <td class="px-4 py-2 border-b">
              <img :src="user.avatar" :alt="user.first_name" class="avatar" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="flex justify-between mt-4">
      <button
        @click="prevPage"
        :disabled="page === 1"
        class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded"
      >
        Previous
      </button>
      <button
        @click="nextPage"
        :disabled="page === totalPages"
        class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded"
      >
        Next
      </button>
    </div>
  </div>
</template>

<style scoped>
.avatar {
  border-radius: 50%;
  width: 50px;
  height: 50px;
}
</style>
