<template>
  <div class="shadow-md flex flex-col">
    <div :class="color">
      <p
        :class="[
          'text-lg font-medium',
          localTask.isDone && 'line-through decoration-red-500 decoration-2',
        ]"
      >
        {{ task.title }}
      </p>

      <input
        type="checkbox"
        class="w-4 mr-3 accent-blue-500"
        v-model="localTask.isDone"
      />
    </div>
    <p
      :class="[
        'p-2 text-sm bg-slate-100',
        localTask.isDone && 'line-through decoration-red-500 decoration-2',
      ]"
    >
      {{ task.desc }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      default() {
        return {};
      },
    },
    color: String,
  },
  computed: {
    localTask: {
      get() {
        return this.task;
      },
      set(newValue) {
        this.$emit("update:task", newValue);
      },
    },
  },
};
</script>
