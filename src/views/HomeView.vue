<script setup lang="ts">
  // importing dependencies
  import logo from "../assets/Netflix_Symbol_RGB.png"
  import Card from "../components/card-component.vue"
  import { ref } from 'vue';
  import axios from 'axios';
import { stat } from "fs";
  const querye =  ref('')
  const data = ref([]);
  const dataCard = ref([])
  let state = ref(false)
  // func to call the api
  async function getTitles() {
    const options = {
    method: 'GET',
    url: 'https://netflix54.p.rapidapi.com/search/',
    params: {
        query: querye.value,
        offset: '0',
        limit_titles: '12',
        limit_suggestions: '5',
        lang: 'en'
      },
      headers: {
        'X-RapidAPI-Key': 'd8dcd1a415mshd1dab5db95986edp1633e2jsncb045f2bf9bc',
        'X-RapidAPI-Host': 'netflix54.p.rapidapi.com'
      }
    };
    if(querye.value.trim() != ''){
      state.value = true
      try {
        const response = await axios.request(options);
        console.log(response)
        data.value = response.data.titles;
        dataCard.value = response.data.suggestions
        // err handling
      } catch (error) {
        console.error(error);
      }
    }
    else{
      state.value = false
      data.value = []
    }
  }
  const onClick = () => {
    state.value = false
  }
</script>
<!-- main html -->
<template>
  <Suspense>
    <template #default>
      <main>
        <!-- bases for the main application -->
        <section>
          <nav class="search-container">
            <div>
              <input v-model="querye" type="text" @input="getTitles" placeholder="Search"/>
              <button @click="onClick">
                search
              </button>
            </div>
          </nav>
          <div :class="state == false ? 'hidden' : 'data'">
            <p v-for="titlea in data.slice(0,5)" :key="titlea.id">
              {{ titlea.jawSummary.title }}
            </p>
            <!-- titlea.jawSummary.backgroundImage.url -->
          </div>
        </section>
        <div class="main-container">
          <Card v-for="titlea in data" :key="titlea.id" :title="titlea.jawSummary.title" :img-src="titlea.jawSummary.backgroundImage.url == null || titlea.jawSummary.backgroundImage.url == '' ? 'https://fireship.io/img/default-cover.webp' : titlea.jawSummary.backgroundImage.url"/>
        </div>
        <!-- <div v-else>

        </div> -->
      </main>
    </template>
    <template #fallback>
      Loading...
    </template>
  </Suspense>
</template>

