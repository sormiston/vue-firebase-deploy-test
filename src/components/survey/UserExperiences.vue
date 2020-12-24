<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="getExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && errorMessage"> {{ errorMessage }}</p>
      <p v-else-if="!isLoading && results.length === 0">No surveys have been submitted yet.  Write a survey review above to be the first!</p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
import axios from '../../axios.js';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      errorMessage: null
    };
  },
  methods: {
    async getExperiences() {
      this.isLoading = true;
      try {
        const response = await axios('/surveys.json');
        const items = [];
        for (let key in response.data) {
          items.push(response.data[key]);
        }
        this.results = items;
      } catch (err) {
        console.error(err)
        this.errorMessage = "Network Error, please try again later."
      } finally {
        this.isLoading = false;
      }
    },
  },
  mounted() {
    this.getExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>