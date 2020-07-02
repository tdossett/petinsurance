<template>
  <v-layout>
    <v-flex class="justify-center">
     <!--  <div class="text-center">
        <pet-image />
      </div> -->
      
      <v-card>
        <v-card-title class="headline">
          Nationwide Pet Insurance enrollment
        </v-card-title>
        <v-card-text>
          <p>Please provide us with your pet information, so we can futher assist you.</p>

          <pet-questions />
          
          <p>Thank you contacting Nationwide Pet Insurance.</p>
          <div class="text-xs-right">
            <em><small>&mdash; NerdBrains</small></em>
          </div>
          <hr class="my-3">
          <a
            href="https://nuxtjs.org/"
            target="_blank"
            rel="noopener noreferrer"
          >
            Nerd Brains Documentation
          </a>
          <br>
          <a
            href="https://github.com/nuxt/nuxt.js"
            target="_blank"
            rel="noopener noreferrer"
          >
            Nerd Brains GitHub
          </a>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            nuxt
            to="/"
          >
            Submit
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import PetQuestions from '~/components/PetQuestions.vue'

export default {
  components: {
    PetQuestions
  },
  data: () => ({
    //
  })
}
</script>