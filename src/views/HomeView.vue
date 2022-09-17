<template>
  <div class="p-4 flex flex-col items-center">
    <h1 class="text-3xl font-bold mb-1 text-white">My Recipes</h1>
    <button
      @click="togglePopup"
      class="bg-emerald-600 rounded-sm px-2 py-1 hover:bg-emerald-700 active:bg-emerald-800 my-2 font-semibold text-white"
    >
      Add new Recipe
    </button>

    <div class="grid grid-cols-3">
      <div
        class="p-4 rounded-sm m-4 bg-blue-800/20"
        v-for="recipe in $store.state.recipes"
        :key="recipe.slug"
      >
        <h2 class="text-2xl text-white font-bold mb-4">{{ recipe.title }}</h2>
        <p class="text-xl font-semibold text-white leading-6 mb-4">
          {{ recipe.description }}
        </p>
        <router-link :to="`/recipe/${recipe.slug}`">
          <button
            class="bg-emerald-600 rounded-sm px-2 py-1 hover:bg-emerald-700 active:bg-emerald-800 text-white font-semibold"
          >
            View Recipe
          </button>
        </router-link>
      </div>
    </div>

    <div
      v-if="popupOpen"
      class="absolute top-0 left-0 w-full h-full bg-black/50 flex justify-center items-center"
    >
      <div class="bg-slate-800 p-8 rounded-2xl w-full max-w-3xl">
        <h2 class="text-2xl mb-4 font-bold text-white">Add new recipe</h2>

        <form @submit.prevent="addNewRecipe">
          <div class="mb-4">
            <label class="block mb-2 text-white font-semibold">Title</label>
            <input
              v-model="newRecipe.title"
              class="block w-full p-2 border rounded-sm mb-4"
              type="text"
            />
          </div>

          <div class="mb-4">
            <label class="block mb-2 text-white font-semibold"
              >Description</label
            >
            <textarea
              v-model="newRecipe.description"
              class="block w-full p-2 border rounded-sm mb-4"
            ></textarea>
          </div>

          <div class="mb-4">
            <label class="block mb-2 text-white font-semibold"
              >Ingredients</label
            >
            <div v-for="i in newRecipe.ingredientRows" :key="i">
              <input
                v-model="newRecipe.ingredients[i - 1]"
                class="block w-full p-2 border rounded-sm mb-4"
                type="text"
              />
            </div>
            <button
              type="button"
              @click="addNewIngredient"
              class="bg-emerald-600 rounded-sm px-2 py-1 font-semibold text-white hover:shadow-md hover:bg-emerald-700 active:bg-emerald-800"
            >
              Add Ingredient
            </button>
          </div>

          <div class="mb-4">
            <label class="block mb-2 text-white font-semibold">Method</label>
            <div v-for="i in newRecipe.methodRows" :key="i">
              <textarea
                v-model="newRecipe.method[i - 1]"
                class="block w-full p-2 border rounded-sm mb-4"
              ></textarea>
            </div>
            <button
              type="button"
              @click="addNewStep"
              class="bg-emerald-600 rounded-sm px-2 py-1 font-semibold text-white hover:shadow-md hover:bg-emerald-700 active:bg-emerald-800"
            >
              Add step
            </button>
          </div>

          <button
            type="submit"
            class="mr-4 bg-emerald-600 rounded-sm px-2 py-1 font-semibold text-white hover:shadow-md hover:bg-emerald-700 active:bg-emerald-800"
          >
            Add Recipe
          </button>
          <button
            @click="togglePopup"
            class="bg-red-500 rounded-sm px-2 py-1 font-semibold text-white hover:shadow-md hover:bg-red-600 active:bg-red-700"
          >
            Close
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { useStore } from "vuex";
import { ref } from "vue";
export default {
  name: "HomeView",

  setup() {
    const newRecipe = ref({
      title: "",
      description: "",
      ingredients: [],
      method: [],
      ingredientRows: 1,
      methodRows: 1,
    });
    const popupOpen = ref(false);
    const store = useStore();

    function togglePopup() {
      popupOpen.value = !popupOpen.value;
    }

    function addNewIngredient() {
      newRecipe.value.ingredientRows++;
    }

    function addNewStep() {
      newRecipe.value.methodRows++;
    }

    function addNewRecipe() {
      newRecipe.value.slug = newRecipe.value.title
        .toLowerCase()
        .replace(/\s/g, "-");

      if (!newRecipe.value.slug) {
        alert("Please enter a title");
        return;
      }
      store.commit("ADD_RECIPE", { ...newRecipe.value });

      newRecipe.value = {
        title: "",
        description: "",
        ingredients: [],
        method: [],
        ingredientRows: 1,
        methodRows: 1,
      };

      togglePopup();
    }
    return {
      newRecipe,
      popupOpen,
      togglePopup,
      addNewIngredient,
      addNewStep,
      addNewRecipe,
    };
  },
};
</script>
