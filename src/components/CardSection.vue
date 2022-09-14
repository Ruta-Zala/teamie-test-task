<template>
  <div class="w-full max-w-full bg-white rounded-lg border border-gray-200 shadow-md flex flex-col">
    <div class="flex justify-end px-4 pt-4">
      <button
          class="inline-block text-gray-500 bg-gray-100 hover:bg-red-100 hover:text-red-500 focus:outline-none rounded-lg text-sm p-1.5"
          type="button" @click="deleteUser(index)">
        <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-trash3-fill h-4 w-4"
             viewBox="0 0 16 16">
          <path
              d="M11 1.5v1h3.5a.5.5 0 0 1 0 1h-.538l-.853 10.66A2 2 0 0 1 11.115 16h-6.23a2 2 0 0 1-1.994-1.84L2.038 3.5H1.5a.5.5 0 0 1 0-1H5v-1A1.5 1.5 0 0 1 6.5 0h3A1.5 1.5 0 0 1 11 1.5Zm-5 0v1h4v-1a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0-.5.5ZM4.5 5.029l.5 8.5a.5.5 0 1 0 .998-.06l-.5-8.5a.5.5 0 1 0-.998.06Zm6.53-.528a.5.5 0 0 0-.528.47l-.5 8.5a.5.5 0 0 0 .998.058l.5-8.5a.5.5 0 0 0-.47-.528ZM8 4.5a.5.5 0 0 0-.5.5v8.5a.5.5 0 0 0 1 0V5a.5.5 0 0 0-.5-.5Z"/>
        </svg>
      </button>
    </div>
    <div class="flex flex-col items-center flex-1">
      <img class="mb-3 w-24 h-24 rounded-full shadow-lg" :src="user.image" :alt="user.image">
      <h5 class="mb-1 text-xl font-bold text-green-900"> {{ user.title }}</h5>
      <h6 class="mb-1 text-md">Twubric Score: <span class="pl-1 font-bold">{{ user.twubric.total }}</span></h6>
      <span class="text-sm text-gray-500">{{ formatDate(user.join_date) }}</span>
      <div class="flex mt-4 space-x-3 md:mt-6 bg-green-900 w-full text-white justify-center p-4 rounded-bl-lg rounded-br-lg flex-1">
        <div>
          Friends:<span class="pl-1 font-bold">{{ user.twubric.friends }}</span>
        </div>
        <div>
          Influence:<span class="pl-1 font-bold">{{ user.twubric.influence }}</span>
        </div>
        <div>
          Chirpiness:<span class="pl-1 font-bold">{{ user.twubric.chirpiness }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  props: {
    user: {
      type: Object,
      required: true,
    },
    deleteUser: Function,
    index: Number
  },

  methods: {
    formatDate(strDate) {
      return moment(strDate).format("DD-MM-YYYY")
    },
  },
}
</script>
