<script setup>
import { onMounted, ref } from 'vue';
import NewsContainer from './components/NewsContainer.vue';

const apiKey = import.meta.env.VITE_API_KEY;
const apiUrl = import.meta.env.VITE_API_URL;

const articles = ref([]);
const nextPage = ref('');


const fetchData = async (page = null) => {
  try {
    const url = new URL(`${apiUrl}?apiKey=${apiKey}`);
    if (page) {
      url.searchParams.append('page', page);
    }

    const response = await fetch(url, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
      },
    });

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    const data = await response.json();
    articles.value = data.results;
    nextPage.value = data.nextPage;
  } catch (error) {
    console.error(error);
    throw new Error('Failed to fetch data');
  }
};

const loadNextPage = () => {
  if (nextPage.value) {
    fetchData(nextPage.value);
  }
};

onMounted(() => {
  fetchData();
});
</script>

<template>
  <main>
    <div class="py-6">
      <h1 class="text-center text-4xl font-bold">News API</h1>
    </div>

    <NewsContainer :articles="articles"  @load-more="loadNextPage" />

  </main>
</template>
