<script setup>
defineProps({
    article: {
        type: Object,
        required: true,
    }
})

</script>

<template>

       
    <article class="flex flex-col items-start justify-between">
          <div class="relative w-full none lg:block">
            
            <img v-if="article.image_url" :src="article.image_url" alt="" class="aspect-video w-full rounded-2xl bg-gray-100 object-cover sm:aspect-[2/1] lg:aspect-[3/2]" />
            <img v-else src="/noimage.jpg" alt="no image"  class="aspect-video w-full rounded-2xl bg-gray-100 object-cover sm:aspect-[2/1] lg:aspect-[3/2]">

            <div class="absolute inset-0 rounded-2xl ring-1 ring-inset ring-gray-900/10" />
          </div>
          <div class="max-w-xl">
            <div class="mt-8 flex items-center gap-x-4 text-xs">
              <time :datetime="article.pubDate" class="text-gray-500">{{ article.pubDate }}</time>
              <p v-for="(category, index) in article.category" :key="index" class="relative z-10 rounded-full bg-gray-50 px-3 py-1.5 font-medium text-gray-600 hover:bg-gray-100">{{ category }}</p>
            </div>
            <div class="group relative">
              <h3 class="mt-3 text-lg/6 font-semibold text-gray-900 group-hover:text-gray-600">
                <a :href="article.link" target="_blank">
                  <span class="absolute inset-0" />
                  {{ article.title }}
                </a>
              </h3>
              <p class="mt-5 line-clamp-3 text-sm/6 text-gray-600">{{ article.description }}</p>
            </div>
            <div class="relative mt-8 flex items-center gap-x-4">
              <div class="text-sm/6">
                <p class="font-semibold text-gray-900">
                  <span v-for="(author, index) in article.author" :key="index">
                    <span class="absolute inset-0" />
                    {{ author }}
                  </span>
                </p>
              </div>
            </div>
          </div>
        </article>
</template>