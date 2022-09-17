<template>
  <div class="p-4 max-w-3xl mx-auto">
    <router-link to="/" class="text-green-500 text-xl font-semibold"
      >&lt;Back</router-link
    >
    <h1 class="text-white font-bold text-3xl mb-4">{{ recipe.title }}</h1>
    <p class="text-xl mb-4 text-white">{{ recipe.description }}</p>
    <hr class="mb-4" />

    <div class="p-4 bg-gray-800 rounded-sm mb-8">
      <h3 class="mb-4 text-white font-semibold text-2xl">Ingredients</h3>
      <ul>
        <li
          class="list-disc list-inside mb-4 text-white"
          v-for="(ingredient, i) in recipe.ingredients"
          :key="i"
        >
          {{ ingredient }}
        </li>
      </ul>
    </div>

    <div>
      <h3 class="mb-4 text-white font-semibold text-2xl">Method</h3>
      <ol>
        <li
          class="mb-8 pb-4 list-inside list-decimal border-b border-gray-500 text-white"
          v-for="(step, i) in recipe.method"
          :key="i"
        >
          <span v-html="cleanText(step)"></span>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    recipe() {
      return this.$store.state.recipes.find(
        (recipe) => recipe.slug === this.$route.params.slug
      );
    },
  },

  methods: {
    cleanText(text) {
      return text.replace(/\n/g, "<br/>");
    },
  },
};
</script>
