<template>
  <div
    class="flex flex-col w-full max-w-sm shadow-md border-2 rounded-lg gap-4"
  >
    <img :src="article.img" :alt="article.title" class="rounded-t-lg h-48" />
    <div class="px-4 pb-4 flex flex-col gap-4">
      <h1 class="text-xl font-bold">{{ article.title }}</h1>
      <NuxtLink :to="'tugas5/article/' + article.id">
        <button
          class="rounded bg-sky-400 w-32 px-1 py-1 self-center font-semibold"
        >
          See Details
        </button>
      </NuxtLink>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    article: {
      type: Object,
      default() {
        return {};
      },
    },
  },
};
</script>
