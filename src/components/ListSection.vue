<template>
    <div>
        <div v-if="users.length === 0"
             class="bg-white w-full h-96 shadow-md rounded-lg flex justify-center items-center text-2xl font-medium">
            <div>
                Nothing Found Trying search again
            </div>
        </div>
        <div v-else
             class="grid grid-cols-1 gap-4 mt-4 md:grid-cols-2 lg:grid-cols-3 2xl:grid-cols-4 justify-items-center">
            <CardSection v-for="(user, index) in NewUsers" :key="index" :index="index" :user="user"
                      :deleteUser="deleteUser"/>
        </div>
    </div>
</template>

<script>
  import CardSection from "./CardSection.vue";

  export default {
    props: {
      users: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        NewUsers: this.users,
      };
    },
    components: {
      CardSection
    },
    methods: {
      deleteUser: async function (index) {
        this.NewUsers.splice(index, 1);
        return this.NewUsers
      }
    }
  }
</script>
