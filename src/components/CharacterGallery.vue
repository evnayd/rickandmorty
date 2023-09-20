<template>
  <h1>gallery</h1>
  <ul class="">
    <li class="" v-for="character in characters" :key="character.id">
      <img class="" v-if="character.image" :src="character.image" />
      <div class="">
        <div>
          <div class="">
            <p class="">{{ character.name }}</p>
            <div>
              <p class="">{{ character.gender }}</p>
              <p class="">{{ character.status }}</p>
            </div>
          </div>
          <h2 class="">
            {{ formatDate(character.created, "DD, MMMM, YYYY") }}
          </h2>
          <p class="">{{ character.location.name }}</p>
        </div>
      </div>
    </li>
  </ul>
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
