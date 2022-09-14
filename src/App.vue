<template>
  <LayoutSection>
    <div class="flex item-center xl:items-end justify-between my-3 flex-col xl:flex-row gap-2">
      <FilterSection v-model="section" :fetch="filterDataBySection" :desc="desc" :currentIndex="currentIndex"/>
      <div class="flex sm:items-end gap-2 flex-col sm:flex-row">
        <div>
          <label>Joined Twitter between</label>
          <Datepicker v-model="startDate" placeholder="Start Date"></Datepicker>
        </div>
        <div>
          <Datepicker v-model="endDate" placeholder="End Date"></Datepicker>
        </div>
        <button @click="filterDataByDate(startDate, endDate)"
                class="px-6 py-2 text-white bg-green-700 rounded hover:bg-green-900"> Filter
        </button>
      </div>
    </div>
    <div>
      <ListSection v-if="!loading && !error" :users="users" />
      <!-- Start of loading animation -->
      <div class="mt-40" v-if="loading">
        <p class="text-6xl font-bold text-center text-gray-500 animate-pulse">
          Loading...
        </p>
      </div>
      <!-- End of loading animation -->

      <!-- Start of error alert -->
      <div class="mt-12 bg-red-50" v-if="error">
        <h3 class="px-4 py-1 text-4xl font-bold text-white bg-red-800">
          {{ error.title }}
        </h3>
        <p class="p-4 text-lg font-bold text-red-900">{{ error.message }}</p>
      </div>
    </div>
  </LayoutSection>
</template>

<script>
import axios from "axios";
import LayoutSection from "./components/LayoutSection.vue";
import FilterSection from "./components/FilterSection.vue";
import ListSection from "./components/ListSection.vue";
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

export default {
  components: {
    LayoutSection,
    FilterSection,
    ListSection,
    Datepicker
  },
  data() {
    return {
      section: "home",
      initialData: [],
      users: [],
      loading: false,
      error: null,
      isActive: false,
      desc: true,
      startDate: null,
      endDate: null,
      currentIndex: 0
    };
  },
  methods: {
    fetchUsers: async function () {
      try {
        this.error = null;
        this.loading = true;
        const url = `https://gist.githubusercontent.com/pandemonia/21703a6a303e0487a73b2610c8db41ab/raw/82e3ef99cde5b6e313922a5ccce7f38e17f790ac/twubric.json`;
        const response = await axios.get(url);
        const results = response.data;
        this.users = results.map((user) => ({
          title: user.fullname,
          image: user.image,
          join_date: user.join_date,
          twubric: user.twubric,
          twubric_score: user.twubric.total,
          friends: user.twubric.friends,
          influence: user.twubric.influence,
          chirpiness: user.twubric.chirpiness,
          uid: user.uid,
          username: user.username
        }));
        this.initialData = this.users;
      } catch (err) {
        if (err.response) {
          // client received an error response (5xx, 4xx)
          this.error = {
            title: "Server Response",
            message: err.message
          };
        } else if (err.request) {
          // client never received a response, or request never left
          this.error = {
            title: "Unable to Reach Server",
            message: err.message
          };
        } else {
          // There's probably an error in your code
          this.error = {
            title: "Application Error",
            message: err.message
          };
        }
      }
      this.loading = false;
    },
    filterDataByDate: async function (startDate, endDate) {
      this.users = this.initialData.filter(user => {
        const startDateUser = new Date(startDate).getTime();
        const endDateUser = new Date(endDate).getTime();
        if (startDateUser < user.join_date && endDateUser > user.join_date) {
          return true
        }
      });
    },
    filterDataBySection: async function (section,index) {
      this.currentIndex = index;
      this.desc = !this.desc;
      this.users = this.initialData.sort((a, b) => {
        if (this.desc) {
         return  a[section] < b[section] ? -1 : 1
        }
        else {
         return a[section] > b[section] ? -1 : 1
        }
      }
      );
    },
  },
  mounted() {
    this.fetchUsers();
  }
};
</script>
