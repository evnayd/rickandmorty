<template>
  <div ref="scrollContainer" class="infinite-scroll-container">
    <character-gallery :characters="computedCharacters"></character-gallery>
    <p v-if="isFinished" class="text-center mb-20 font-medium text-xl">
      Congratulations! You've scrolled to the end.<br />
      There are no more characters in Rick and Morty series.
    </p>
    <top-button v-if="isButtonVisible" @goTop="handleGoTop"></top-button>
  </div>
</template>

<script>
import CharacterGallery from "./components/CharacterGallery.vue";
import TopButton from "./components/TopButton.vue";
import { ref, onMounted, onUnmounted, computed } from "vue";

export default {
  name: "App",
  components: { CharacterGallery, TopButton },
  setup() {
    const characters = ref([]);
    const pageNumber = ref(1);
    const isFinished = ref(false);
    const isButtonVisible = ref(false);
    const maxCharacters = ref(0);

    // Fetching data from the server
    const fetchData = async () => {
      try {
        if (
          maxCharacters.value > 0 &&
          characters.value.length >= maxCharacters.value
        ) {
          console.log("The end. All characters are fetched.");
          isFinished.value = true;
          return;
        }

        const response = await fetch(
          `https://rickandmortyapi.com/api/character/?page=${pageNumber.value}`
        );
        const result = await response.json();

        if (result.results) {
          characters.value = characters.value.concat(result.results);

          pageNumber.value++;

          // Updating the maximum characters count
          maxCharacters.value = result.info.count;
        } else {
          console.log("No more characters available.");
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    const doScroll = (event) => {
      const scrollHeight = event.target.scrollHeight;
      const scrollTop = event.target.scrollTop;
      const clientHeight = event.target.clientHeight;

      if (scrollTop > clientHeight / 2) {
        //Showing the button if the user has scrolled more than half of the client height
        isButtonVisible.value = true;
      } else {
        isButtonVisible.value = false;
      }

      if (scrollTop + clientHeight >= scrollHeight) {
        fetchData();
      } else {
        return;
      }
    };

    const handleGoTop = () => {
      const container = document.querySelector(".infinite-scroll-container");
      container.scrollTo({
        top: 0,
        behavior: "smooth",
      });
    };

    onMounted(() => {
      fetchData();
      const container = document.querySelector(".infinite-scroll-container");
      container.addEventListener("scroll", doScroll);
    });

    // Removing the scroll event listener when the component is unmounted
    onUnmounted(() => {
      const container = document.querySelector(".infinite-scroll-container");
      container.removeEventListener("scroll", doScroll);
    });

    const computedCharacters = computed(() => characters.value);

    return {
      computedCharacters,
      isFinished,
      isButtonVisible,
      handleGoTop,
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
