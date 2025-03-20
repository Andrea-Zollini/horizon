<script setup>
import { ref, computed } from 'vue';
import { languages, categories, countries } from '../data.js';

import Article from './Article.vue';
import Select from './ui/Select.vue';

const props = defineProps({
    articles: {
        type: Array,
        required: true,
    },
});

const emit = defineEmits(['load-more']);

const selectedLanguage = ref('');
const selectedCategory = ref('');
const selectedCountry = ref('');


const filteredArticles = computed(() => {
    return props.articles.filter((article) => {
        return (
            (!selectedLanguage.value || article.language.toLowerCase().includes(selectedLanguage.value.toLowerCase())) &&
            (!selectedCategory.value || article.category == selectedCategory.value) &&
            (!selectedCountry.value || article.country.includes(selectedCountry.value.toLowerCase()))
        );
    });
});


const handleLanguageChange = (string) => {
    selectedLanguage.value = string;
}

const handleCategoryChange = (string) => {
    selectedCategory.value = string;
}

const handleCountryChange = (string) => {
    selectedCountry.value = string;
}

const resetFilters = () => {
    selectedLanguage.value = '';
    selectedCategory.value = '';
    selectedCountry.value = '';
}

const loadNextPage = () => {
    emit('load-more');
};

</script>

<template>

    <div class="lg:flex px-3 py-3 lg:px-6">
        <!-- Filter Section -->
        <div id="filter-section"
            class="bg-gray-200 mx-auto max-w-xl lg:mx-0 max-h-fit shadow-md rounded-xl p-4 lg:p-8  lg:min-w-sm">
            <Select id="language" label="Language" :items="languages" valueKey="code" labelKey="language"
                @updateValue="handleLanguageChange" v-model="selectedLanguage" placeholder="Select a language" />

            <Select id="category" label="Category" v-model="selectedCategory" :items="categories" valueKey=""
                labelKey="" @updateValue="handleCategoryChange" placeholder="Select a category" />
            <Select id="country" label="Country" v-model="selectedCountry" :items="countries" valueKey="code"
                labelKey="country" @updateValue="handleCountryChange" placeholder="Select a country" />

            <div class="flex justify-center items-center py-3">
                <button @click="resetFilters" class="bg-black text-white p-3 rounded-lg cursor-pointer">
                    Reset all filters
                </button>
                <button @click="loadNextPage" class="bg-black text-white p-3 rounded-lg cursor-pointer ms-3">
                    Next Page
                </button>
            </div>
        </div>

        <!-- Content Section -->
        <div id="content-section" class="flex flex-col items-center gap-8 lg:px-8">
            <div v-if="filteredArticles.length === 0" class="pt-6">
                <p class="text-2xl font-semibold">No articles found</p>
            </div>

            <div v-else
                class="mx-auto mt-8 lg:mt-0 grid max-w-2xl grid-cols-1 gap-x-8 gap-y-20 lg:mx-0 lg:max-w-none sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5">
                <Article v-for="article in filteredArticles" :key="article.id" :article="article" />
            </div>
        </div>
    </div>
</template>