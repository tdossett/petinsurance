<template>
  <v-layout>
    <v-flex class="justify-center">
      <v-card class="pa-2">
        <div class="text-center">
          <pet-image />
        </div>
        <blockquote class="blockquote text-center">
          &#8220;First, solve the problem. Then, write the code.&#8221;
          <footer>
            <small>
              <em>&mdash;John Johnson</em>
            </small>
          </footer>
        </blockquote>
      </v-card>

      <v-card>
        <v-card-title class="headline">
          Welcome to Nerd Brains team from Nationwide Pet Insurance.
        </v-card-title>
        <v-card-text>
          <p>We've developed an AI/ML application that provides new NW Pet Insurance insights .</p>
          <p>
            For more information on Nerd Brains, check out the <a
              href="https://vuetifyjs.com"
              target="_blank"
              rel="noopener noreferrer"
            >
              documentation
            </a>.
          </p>
          <p>
            If you have questions, please join the official <a
              href="https://chat.vuetifyjs.com/"
              target="_blank"
              rel="noopener noreferrer"
              title="chat"
            >
              discord
            </a>.
          </p>
          <p>
            Find a bug? Report it on the github <a
              href="https://github.com/vuetifyjs/vuetify/issues"
              target="_blank"
              rel="noopener noreferrer"
              title="contribute"
            >
              issue board
            </a>.
          </p>
          <p>Thank you and we look forward to bringing more exciting features in the future.</p>
          <div class="text-xs-right">
            <em><small>&mdash; Team: .................</small></em>
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
            color="blue darken-1"
            text
            nuxt
            to="/enroll"
          >
            Continue
          </v-btn>
        </v-card-actions>
      </v-card>

      
    </v-flex>
  </v-layout>
</template>

<script>
import PetImage from '~/components/PetImage.vue'

export default {
  components: {
    PetImage
  }
}
</script>