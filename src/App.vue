<template>
  <div ref="scrollContainer" class="infinite-scroll-container">
    <character-gallery :characters="computedCharacters"></character-gallery>
  </div>
</template>

<script>
import CharacterGallery from "./components/CharacterGallery.vue";
import { ref, onMounted, onUnmounted, computed } from "vue";

export default {
  name: "App",
  components: { CharacterGallery },
  setup() {
    const characters = ref([]);
    const pageNumber = ref(1);
    const isLoading = ref(false);
    const bottom = ref(false);

    // Fetching data from the server
    const fetchData = async () => {
      try {
        const response = await fetch(
          `https://rickandmortyapi.com/api/character/?page=${pageNumber.value}`
        );
        const result = await response.json();

        if (result.results) {
          characters.value = characters.value.concat(result.results);

          pageNumber.value++;
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        isLoading.value = false;
      }
    };

    const doScroll = (event) => {
      const scrollHeight = event.target.scrollHeight;
      const scrollTop = event.target.scrollTop;
      const clientHeight = event.target.clientHeight;

      if (scrollTop + clientHeight >= scrollHeight) {
        bottom.value = true;
        fetchData();
      } else {
        bottom.value = false;
      }
    };

    onMounted(() => {
      fetchData();
      const container = document.querySelector(".infinite-scroll-container");
      container.addEventListener("scroll", doScroll);
    });

    // Removing the scroll event listener when the component is unmounted
    onUnmounted(() => {
      const container = document.querySelector(".infinite-scroll-container");
      container.addEventListener("scroll", doScroll);
    });

    const computedCharacters = computed(() => characters.value);

    return {
      computedCharacters,
    };
  },
};
</script>

<style scoped>
.infinite-scroll-container {
  height: 2500px;
  overflow-y: scroll;
  padding-left: 10px;
  padding-right: 10px;
}
</style>
