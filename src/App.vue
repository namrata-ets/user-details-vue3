<template>
  <q-banner class="bg-white-12">
    <template v-slot:avatar>
      <q-icon class="icon-position" name="people" color="primary" />
    </template>
  <span class="title"> Fetching User Data from a REST API </span>
  <template v-slot:action>
      <q-btn class="button-position" color="primary" :loading="isLoading" label="CLICK HERE" @click="fetchUsers"></q-btn>
    </template>
  </q-banner>

    <q-card flat bordered class="bg-blue-9 my-card">
      <q-card-section>
        <div class="text-h6">User Details</div>
        <div class="text-subtitle2">JSON PlaceHolder API Records</div>
      </q-card-section>

      <q-separator dark inset />

      <q-card-section v-if="users">

        <User :users="users" />

      </q-card-section>
      <q-card-section v-else>
        <div class="text-h6">No data available</div>
      </q-card-section>
    </q-card>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import User from './components/User.vue';
import { Dialog } from 'quasar';

const users = ref([]);
const isLoading=ref(false);
const errorMessage =ref('');

const fetchUsers = async () =>{
  const showErrorDialog = () => {
      Dialog.create({
        title: 'Error',
        message: errorMessage.value,
        color: 'negative',
        persistent: true, // Optional: Makes dialog persistent until closed by user
        ok: {
          label: 'OK',
          flat: true // Optional: Use flat design for the button
        }
      });
    };
  try {
    isLoading.value=true;
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
     //check if the API call is successful status code will be from 200 to 299, else any other status code that indicates API request unsuccessful
      if (!response.status.toString().startsWith('2')) {
          throw new Error(`API error: ${response.status} - ${response.statusText}`);
      }
    users.value = response.data;
    isLoading.value=false;
  }
  catch (error) {
      isLoading.value = false;
      errorMessage.value = error.response.data.message;
        if(errorMessage.value)
          showErrorDialog();
        console.error('Error fetching users:', error);
        if (error.isAxiosError && error.response) {
              //Check for Network errors or other status code errors
              console.error(`Network error (${error.response.status}): ${error.response.data}`);
            } else {
              // Log any other errors
              console.error('Unexpected error:', error);
            }
  }
  finally {
      isLoading.value = false;
    }

};


</script>

<style>
.title {
  font-family: Georgia, 'Times New Roman', Times, serif;
  font-weight: bold;
  color: black;
  font-size: 50px;
  align-items: center;
  padding-left: 100px;
}

.icon-position {
  padding-top: 50px;
}

.button-position {
  margin-right: 150px;
  margin-top: 50px;
  margin-bottom: 40px;
}
</style>
