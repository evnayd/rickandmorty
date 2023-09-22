<template>
  <character-gallery :characters="characters"></character-gallery>
</template>

<script>
import CharacterGallery from "./components/CharacterGallery.vue";
import { ref, onMounted } from "vue";

export default {
  name: "App",
  components: { CharacterGallery },
  setup() {
    const characters = ref([]);

    // Function to fetch data from the server
    const fetchData = async () => {
      try {
        const response = await fetch(
          "https://rickandmortyapi.com/api/character"
        );
        const result = await response.json();
        console.log("result", result);

        characters.value = result.results;
        console.log("characters.value", result.results);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    onMounted(() => {
      fetchData();
    });

    return {
      characters,
    };
  },
};
</script>
