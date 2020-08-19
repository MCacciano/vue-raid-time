<template>
  <form class="flex flex-col border rounded p-3 border-gray-500" @sumbit.prevent>
    <label for="forename" class="flex my-2">
      <span class="font-light pr-3">Forename</span>
      <input
        v-model="forename"
        id="forename"
        class="bg-gray-200 p-1 focus:bg-white flex-grow mx-2 border border-gray-500 rounded"
      />
    </label>
    <label for="surname" class="flex my-2">
      <span class="font-light pr-5">Surname</span>
      <input
        v-model="surname"
        id="surname"
        class="bg-gray-200 p-1 focus:bg-white flex-grow mx-2 border border-gray-500 rounded"
      />
    </label>
    <div class="flex justify-center items-center my-2">
      <label for="server" class="my-2 flex-1 px-4">
        <select
          class="bg-gray-200 p-1 focus:bg-white w-full border border-gray-500"
          name="server"
          v-model="chosenServer"
        >
          <option class="font-thin" disabled value="">Server</option>
          <option class="font-thin" v-for="(server, i) in servers" :key="i" :value="server">
            {{ server }}
          </option>
        </select>
      </label>
      <div class="flex-1 px-4">
        <button
          type="button"
          @click="handleOnSubmit"
          class="bg-blue-700 hover:bg-blue-800 text-white px-2 py-1 font-normal w-full"
        >
          Submit
        </button>
      </div>
    </div>
    <div class="flex flex-col font-medium whitespace-wrap break-words">
      <small class="text-red-700">
        <span>
          <fa-icon :icon="['fas', 'exclamation-circle']" size="sm" />
        </span>
        Paste this id number into your lodestone character profile before submitting this form.
      </small>

      <p class="text-lg py-1">
        <span class="font-semibold">ID:&nbsp;</span>
        <!-- <span class="font-medium">{{ userProfile.character.linkID }}</span> -->
      </p>
    </div>
  </form>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';

export default {
  name: 'LinkAccountForm',
  data() {
    return {
      forename: '',
      surname: '',
      chosenServer: '',
      results: []
    };
  },
  computed: {
    ...mapGetters('xivapi', ['servers']),
    ...mapGetters('firebase', ['userProfile'])
  },
  methods: {
    ...mapActions('xivapi', ['fetchServers', 'validateCharacter']),
    handleOnSubmit(e) {
      this.validateCharacter({
        name: `${this.forename}+${this.surname}`,
        server: this.chosenServer
      });
    }
  },
  created() {
    this.fetchServers();
  }
};
</script>