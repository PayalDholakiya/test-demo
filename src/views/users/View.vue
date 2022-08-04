<template>
  <div class="p-10 bg-white">
    <div class="flex justify-center flex-col items-center">
      <img class="h-40" :src="selectedUser.image" />
      <h1>Name : {{ selectedUser.name }}</h1>
      <h1>Email :{{ selectedUser.email }}</h1>
      <h1>Phone : {{ selectedUser.mobile }}</h1>

      <div>Address : {{ selectedUser.address }}</div>
      <button
        type="button"
        class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        @click="$router.push('/')"
      >
        Back to users
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      selectedUser: null,
    }
  },

  mounted() {
    if (this.$route.params.id) {
      this.fetchData()
    }
  },
  methods: {
    fetchData() {
      axios
        .get(`http://localhost:3000/users/${this.$route.params.id}`)
        .then((res) => {
          this.selectedUser = res.data
        })
    },
  },
}
</script>
