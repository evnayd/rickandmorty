<template>
  <div class="container mx-auto py-20">
    <h1 class="text-center mb-20 font-medium text-4xl">
      Welcome to Rick and Morty character gallery!
    </h1>
    <ul class="flex flex-wrap sm:justify-between justify-center">
      <li
        class="mb-12 mx-1 bg-white rounded-lg shadow-md p-8"
        v-for="character in characters"
        :key="character.id"
      >
        <img class="" v-if="character.image" :src="character.image" />
        <div class="pt-4">
          <h2 class="uppercase font-bold">{{ character.name }}</h2>
          <p class="">{{ character.gender }}</p>
          <p class="">{{ character.status }}</p>
          <p class="">
            {{ formatDate(character.created, "DD, MMMM, YYYY") }}
          </p>
          <p class="">{{ character.location.name }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import moment from "moment";

export default defineComponent({
  props: ["characters"],

  setup() {
    const formatDate = (dateString, format) => {
      return moment(dateString).format(format);
    };

    return {
      formatDate,
    };
  },
});
</script>
