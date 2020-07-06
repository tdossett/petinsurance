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
            label="Pet name"
            required
          ></v-text-field>
        </v-col>

        <v-col
          cols="4"
          md="4"
        >
          <v-select
            v-model="select2"
            clearable
            :items="age"
            label="Age"
          ></v-select>
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
          <v-combobox
            v-model="select1"
            v-bind:items="items1"
            item-text="stateName"
            item-value="stateAbrev"
            label="Select Location"
            :rules="locationRules"
          ></v-combobox>
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
      <v-divider></v-divider>
      <v-card-text class="mt-2">
        <v-btn color="blue darken-1" text>submit</v-btn>
        <v-btn color="blue darken-1" text>clear</v-btn>
      </v-card-text>
    </v-container>
  </v-form>
</template>
<script>
  export default {
      data: () => ({
        valid: false,
        items: [],
        items1: [],
        firstname: '',
        location: '',
        nameRules: [
          v => !!v || 'Pet Name is required',
          v => v.length <= 10 || 'Name must be less than 10 characters',
        ],
        locationRules: [
          v => !!v || 'Location is required',
        ],
        email: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        select: null,
        select1: null,
        select2: null,
        age: [1,2,3,4,5,6,7,8,9,10]
      }),
      mounted() {
        this.getData()
        this.getStates()
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
        async getStates() {
          /**
           * Get the state data
           */
          const stateDataReq = await fetch('/states.json')
          const stateData = await stateDataReq.json()
          const items1 = stateData.map(state => ({
            stateName: state.name,
            stateAbrev: state.abbreviation
          }))

          console.log('items1', items1)

          this.items1 = items1
        },
      }
    }
</script>