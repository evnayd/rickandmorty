<template>
  <div class="container mx-auto py-20">
    <h1 class="text-center mb-20 font-medium text-2xl md:text-4xl w-full">
      Welcome to the Rick and Morty character gallery!
    </h1>
    <ul class="flex flex-wrap sm:justify-between justify-center">
      <li
        class="mb-12 mx-1 bg-white rounded-lg shadow-md p-8 w-full md:w-80"
        v-for="character in characters"
        :key="character.id"
      >
        <img
          class="w-full object-cover bg-red-200 h-64 bg-slate-50"
          alt="character image"
          v-if="character.image"
          :src="character.image"
        />
        <div class="">
          <h2 class="uppercase font-bold py-4 w-full">
            {{ character.name }}
          </h2>
          <div class="flex items-baseline">
            <font-awesome-icon
              :icon="['fas', 'user']"
              class="mr-4"
              style="color: #e0e0e1"
            />
            <p class="">{{ character.gender }}</p>
          </div>
          <div class="flex items-baseline">
            <font-awesome-icon
              :icon="['fas', 'check']"
              class="mr-4"
              style="color: #e0e0e1"
            />
            <p class="">{{ character.status }}</p>
          </div>
          <div class="flex items-baseline">
            <font-awesome-icon
              :icon="['fas', 'calendar-check']"
              class="mr-4"
              style="color: #e0e0e1"
            />
            <p class="">
              {{ formatDate(character.created, "DD, MMMM, YYYY") }}
            </p>
          </div>
          <div class="flex items-baseline">
            <font-awesome-icon
              :icon="['fas', 'location-dot']"
              class="mr-4"
              style="color: #e0e0e1"
            />
            <p class="">{{ character.location.name }}</p>
          </div>
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
