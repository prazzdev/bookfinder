<script setup>
import Navbar from "./components/Navbar.vue";
import FormSearch from "./components/FormSearch.vue";
import Card from "./components/Card.vue";
import Spinner from "./components/Spinner.vue";
import { ref } from "vue";

const books = ref(null);
const isLoading = ref(false);

async function search({ keyword }) {
  if (!keyword) return;
  const url = new URL(import.meta.env.VITE_BASE_URL);
  url.searchParams.set("q", keyword);

  isLoading.value = true;
  const res = await fetch(url);
  const data = await res.json();

  isLoading.value = false;
  books.value = { items: data.items, totalItems: data.totalItems };
}
</script>

<template>
  <Navbar />
  <section class="flex flex-col items-center my-12">
    <h1 class="text-xl md:text-2xl font-semibold mb-6">
      Find the book you want.
    </h1>
    <FormSearch @search="search" />
  </section>
  <div class="w-full flex justify-center items-center" v-if="isLoading">
    <Spinner />
  </div>
  <section class="w-[90vw] md:w-[80vw] mx-auto mb-8" v-else>
    <h1 class="text-xl font-semibold" v-if="books">
      {{ `${books.totalItems} books found.` }}
    </h1>
    <div class="flex flex-col items-center" v-else>
      <img
        src="https://cdni.iconscout.com/illustration/premium/thumb/waiting-9773388-7973911.png"
        alt="waiting"
        class="w-36 h-36 mb-1"
      />
      <p class="">waiting for search</p>
    </div>
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-4" v-if="books">
      <Card v-for="{ volumeInfo, id } of books.items" :key="id">
        <template #header>
          <figure class="">
            <img
              :src="volumeInfo?.imageLinks?.thumbnail"
              alt="book"
              class="w-full h-full md:w-48 object-cover"
            />
          </figure>
        </template>
        <template #content>
          <h3 class="font-bold md:text-lg">{{ volumeInfo?.title }}</h3>
          <p class="text-sm">Author: {{ volumeInfo?.authors?.toString() }}</p>
          <p class="text-sm">
            Publisher: {{ volumeInfo?.publisher?.toString() }}
          </p>
          <p class="text-sm">
            Published Date: {{ volumeInfo?.publishedDate?.toString() }}
          </p>
          <a
            :href="volumeInfo?.infoLink"
            target="_blank"
            rel="noopener"
            class="inline-block py-2 px-4 bg-zinc-100 font-semibold rounded-md hover:bg-zinc-100/90"
            >More Info</a
          >
        </template>
      </Card>
    </div>
  </section>
</template>
