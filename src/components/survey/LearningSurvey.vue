<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <div class="form-control">
          <input
            type="radio"
            id="rating-poor"
            value="poor"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-poor">Poor</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-great"
            value="great"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-great">Great</label>
        </div>
        <p v-if="invalidInput">
          One or more input fields are invalid. Please check your provided data.
        </p>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
      <p v-if="errorMessage">{{ errorMessage }}</p>
      <img v-if="errorCat" :src="errorCat" width="300" height="200" />
    </base-card>
  </section>
</template>

<script>
import axios from '../../axios.js';
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
      errorMessage: null,
      errorCat: null,
    };
  },

  methods: {
    submitSurvey() {
      // reset errors
      this.errorMessage = null;
      this.errorCat = null;
      // validation
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;
      // axios
      this.postExperience();
      // local state clean-up
      this.enteredName = '';
      this.chosenRating = null;
    },
    async postExperience() {
      try {
        await axios.post('/surveys.json', {
          name: this.enteredName,
          rating: this.chosenRating,
        });
      } catch (err) {
        this.errorMessage = err.message;
        if (err.response.status) {
          this.errorMessage =
            'Your data could not be stored.  Please try again later.';
          this.errorCat = `https://http.cat/${err.response.status}`;
        }
      }
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>