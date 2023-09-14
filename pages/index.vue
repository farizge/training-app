<template>
  <div class="min-w-full max-w-md w-full flex-col px-8 py-4">
    <!-- header -->
    <div
      class="border-b border-gray-400 py-4 mb-4 flex flex-wrap gap-2 justify-between items-center"
    >
      <div class="flex flex-row gap-3 items-center">
        <h1 class="font-bold text-2xl">Tasks</h1>
        <!-- add task button -->
        <div class="relative">
          <button
            @click="showForm = true"
            class="px-2 py-1 bg-pink-500 rounded font-medium text-white"
          >
            + Add Task
          </button>
          <!-- form add task -->
          <div
            :class="[
              'absolute border-2 mt-2 p-4 rounded w-[300px] left-0 m-auto bg-white shadow-xl',
              !showForm && 'hidden',
            ]"
          >
            <h1 class="text-center font-bold mb-4">Add Task</h1>
            <button
              @click="showForm = false"
              class="text-lg font-bold absolute top-3 right-4"
            >
              X
            </button>
            <form
              v-on:submit.prevent
              @submit="handleSubmit"
              class="flex flex-col gap-4"
            >
              <input
                type="text"
                v-model="tasks.title"
                placeholder="Title"
                class="border w-full rounded border-black indent-2"
              />
              <textarea
                name="description"
                v-model="tasks.desc"
                cols="30"
                rows="10"
                placeholder="Description"
                class="border border-black w-full rounded indent-2"
              ></textarea>
              <label for="category">
                Category
                <select
                  name="category"
                  id="category"
                  v-model="tasks.category"
                  class="border border-black rounded w-full"
                >
                  <option disabled>--Category--</option>
                  <option value="tugas" class="bg-orange-400">Tugas</option>
                  <option value="belanja" class="bg-lime-400">Belanja</option>
                  <option value="pekerjaan" class="bg-sky-400">
                    Pekerjaan
                  </option>
                </select>
              </label>
              <button
                type="submit"
                class="px-2 py-1 bg-pink-500 rounded font-medium text-white"
              >
                Add Task
              </button>
            </form>
          </div>
        </div>
      </div>

      <!-- search bar  -->
      <input
        type="search"
        placeholder="Cari..."
        class="p-1 rounded indent-3 border border-black focus:border-transparent"
        v-model="searchQuery"
      />
    </div>
    <!-- filter -->
    <div class="flex justify-between lg:justify-end gap-2 mb-4">
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
    <!-- task list -->
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
      searchQuery: "", //query keyword
      selectedCategory: "semua", //filter category
      isGrid: true, //change view
      showForm: false, //show form component
    };
  },
  methods: {
    // color by category
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
    // handle submit form
    handleSubmit() {
      const formTask = {
        id: this.tasks.length + 1,
        title: this.tasks.title,
        desc: this.tasks.desc,
        isDone: false,
        category: this.tasks.category,
      };
      this.tasks.push(formTask);
      this.tasks.title = "";
      this.tasks.desc = "";
      this.tasks.category = "";
      this.showForm = false;
    },
  },
  computed: {
    //filtering list
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
