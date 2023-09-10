<template>
  <div class="min-w-full max-w-md w-full flex-col px-8 py-4">
    <div
      class="border-b border-gray-400 py-4 mb-4 flex justify-between items-center"
    >
      <h1 class="font-bold text-2xl flex-1">Tasks</h1>
      <!-- header -->
      <div class="flex flex-row gap-3 items-center">
        <!-- search bar  -->
        <input
          type="search"
          placeholder="Cari..."
          class="border border-black rounded focus:border-none indent-3"
          v-model="searchQuery"
        />
        <label for="category">
          Category:
          <select
            name="category"
            id="category"
            v-model="selectedCategory"
            class="border border-black rounded"
          >
            <option value="semua" class="">Semua</option>
            <option value="tugas" class="bg-orange-400">Tugas</option>
            <option value="belanja" class="bg-lime-400">Belanja</option>
            <option value="pekerjaan" class="bg-sky-400">Pekerjaan</option>
          </select>
        </label>
        <div class="flex items-center gap-1">
          <p>View:</p>
          <button
            class="border border-black py-1 px-2 leading-none text-sm rounded"
            @click="isGrid = !isGrid"
          >
            {{ isGrid ? "List" : "Grid" }}
          </button>
        </div>
      </div>
    </div>
    <div :class="['flex flex-wrap gap-4', isGrid ? 'flex-row' : 'flex-col']">
      <div class="w-full max-w-md" v-for="task in resultQuery">
        <Task
          :key="task.id"
          :task="task"
          :color="colorCategory(task.category)"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [
        {
          id: 1,
          title: "Belanja Ke Pasar",
          desc: "Belanja ke pasar",
          isDone: true,
          category: "belanja",
        },
        {
          id: 2,
          title: "Ngerjain PR",
          desc: "Ngerjain PR Matematika",
          isDone: false,
          category: "tugas",
        },
        {
          id: 3,
          title: "Bikin 10 Website",
          desc: "Bikin 10 Website",
          isDone: false,
          category: "pekerjaan",
        },
        {
          id: 4,
          title: "Belanja ke Superindo",
          desc: "belanja bulanan",
          isDone: false,
          category: "belanja",
        },
      ],
      searchQuery: "",
      selectedCategory: "semua",
      isGrid: false,
    };
  },
  methods: {
    colorCategory(cat) {
      switch (cat) {
        case "belanja":
          return "flex justify-between p-2 bg-lime-400";
        case "tugas":
          return "flex justify-between p-2 bg-orange-400";
        case "pekerjaan":
          return "flex justify-between p-2 bg-sky-400";
      }
    },
  },
  computed: {
    resultQuery() {
      if (this.searchQuery) {
        return this.tasks.filter((task) => {
          return this.searchQuery
            .toLowerCase()
            .split("")
            .every((q) => task.title.toLowerCase().includes(q));
        });
      } else if (this.selectedCategory !== "semua") {
        return this.tasks.filter(
          (task) => task.category === this.selectedCategory
        );
      } else {
        return this.tasks;
      }
    },
  },
};
</script>
