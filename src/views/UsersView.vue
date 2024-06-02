<script>
import axios from 'axios' // Import axios for making HTTP requests

export default {
  name: 'App', // Name of the component
  data() {
    return {
      users: [], // Array to store user data
      page: 1,
      totalPages: 2 // Total pages set to 2 as per your requirement
    }
  },
  created() {
    // Fetch users when the component is created
    this.fetchUsers()
  },
  methods: {
    // Method to fetch users from the API
    async fetchUsers() {
      try {
        // Make a GET request to the API with the current page number

        const response = await axios.get(`https://reqres.in/api/users?page=${this.page}`)
        this.users = response.data.data
        this.totalPages = response.data.total_pages // Update totalPages dynamically
      } catch (error) {
        // Log any errors that occur during the request
        console.error('Error fetching users:', error)
      }
    },
    // Method to go to the next page
    nextPage() {
      if (this.page < this.totalPages) {
        this.page++
        this.fetchUsers()
      }
    },
    prevPage() {
      if (this.page > 1) {
        this.page--
        this.fetchUsers() // Fetch users for the new page
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
          <!-- Iterate over users array to create table rows -->
          <tr
            v-for="user in users"
            :key="user.id"
            class="hover:bg-gray-100 odd:bg-gray-100 even:bg-white"
          >
            <td class="px-4 py-2 border-b">{{ user.id }}</td>
            <td class="px-4 py-2 border-b">
              {{ user.first_name }} {{ user.last_name }}

              <!-- Email link for contacting the user -->
              <a :href="'mailto:' + user.email" class="text-blue-500 ml-2">(Contact)</a>
            </td>
            <td class="px-4 py-2 border-b">{{ user.email }}</td>
            <td class="px-4 py-2 border-b">
              <!-- Display user avatar -->
              <img :src="user.avatar" :alt="user.first_name" class="avatar" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="flex justify-between mt-4">
      <!-- Button to go to the previous page, disabled if on the first page -->
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
