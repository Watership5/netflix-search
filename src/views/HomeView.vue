<script setup lang="ts">
  // importing dependencies
  import { ref } from 'vue';
  import axios from 'axios';
  const querye =  ref('')
  const data = ref([]);
  // func to call the api
  async function getTitles() {
    const options = {
    method: 'GET',
    url: 'https://netflix54.p.rapidapi.com/search/' + '?nocache=',
    params: {
        query: querye.value,
        offset: '0',
        limit_titles: '10',
        limit_suggestions: '10',
        lang: 'en'
      },
      headers: {
        'X-RapidAPI-Key': 'd8dcd1a415mshd1dab5db95986edp1633e2jsncb045f2bf9bc',
        'X-RapidAPI-Host': 'netflix54.p.rapidapi.com'
      }
    };
    try {
      const response = await axios.request(options);
      console.log(response)
      data.value = response.data.titles;
      querye.value = ''
      // err handling
    } catch (error) {
      console.error(error);
    }
  }
</script>
<!-- main html -->
<template>
  <Suspense>
    <template #default>
      <main>
        <!-- bases for the main application -->
        <input v-model="querye" type="text"/>
        <h1 v-for="titlea in data" :key="titlea.id">
          {{ titlea.jawSummary.title }}
        </h1>
        <button @click="getTitles">
          click me
        </button>
      </main>
    </template>
    <template #fallback>
      Loading...
    </template>
  </Suspense>
</template>

