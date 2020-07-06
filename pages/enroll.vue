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
          <p>Tell us about your pet.</p>

          <pet-questions />
          
        </v-card-text>
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