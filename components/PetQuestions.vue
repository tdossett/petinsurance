<template>
  <v-form v-model="valid">
    <v-container>
      <v-row>

        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="firstname"
            :rules="nameRules"
            :counter="10"
            label="First name"
            required
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-combobox
            v-model="select"
            v-bind:items="items"
            item-text="breedName"
            item-value="breedCode"
            label="Select Breed"
          ></v-combobox>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="lastname"
            :rules="nameRules"
            :counter="10"
            label="Last name"
            required
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-text-field
            v-model="email"
            :rules="emailRules"
            label="E-mail"
            required
          ></v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>
<script>
  export default {
      data: () => ({
        valid: false,
        items: [],
        firstname: '',
        lastname: '',
        nameRules: [
          v => !!v || 'Name is required',
          v => v.length <= 10 || 'Name must be less than 10 characters',
        ],
        email: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        select: null
      }),
      mounted() {
        this.getData()
      },
      methods: {
        async getData() {
          /**
           * Get the pet data reduced to just the variables we are intersted
           * and cleaned of missing data
           */
          const petDataReq = await fetch('/petdata.json')
          const petData = await petDataReq.json()
          const items = petData.map(pet => ({
            breedCode: parseInt(pet.BreedCode),
            breedName: pet.BreedName,
            size: pet.Size
          }))

          console.log('items', items)

          this.items = items
        },
      }
    }
</script>